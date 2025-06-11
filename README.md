# Examen1
Examen numero 1 de progamacion II UH

using System;
using System.Collections.Generic;

namespace RegistroAsistencia
{
    class Program
    {
        // Justificación del enfoque:
        // La primera clave es el día de la semana (Lunes a Viernes).
        // La segunda clave es la materia (Matemáticas, Lengua, etc.).
        // El valor es una lista de nombres de estudiantes (máximo 3 por clase).
        // Este enfoque facilita registrar, eliminar y consultar estudiantes por día y materia de forma sencilla y clara.


        // Días y materias válidas
        static string[] dias = { "Lunes", "Martes", "Miércoles", "Jueves", "Viernes" };
        static string[] materias = { "Matematicas", "Español", "Sociales", "Ciencias", "Ingles" };

        // Diccionario de asistencia: Día -> Materia -> Lista de estudiantes
        static Dictionary<string, Dictionary<string, List<string>>> asistencia = new Dictionary<string, Dictionary<string, List<string>>>();

        static void Main()
        {
            InicializarAsistencia();

            while (true)
            {
                Console.WriteLine("\n--- Sistema de Registro de Asistencia ---");
                Console.WriteLine("1. Registrar estudiante");
                Console.WriteLine("2. Eliminar estudiante");
                Console.WriteLine("3. Consultar estudiantes por materia y día");
                Console.WriteLine("4. Consultar estudiantes por día completo");
                Console.WriteLine("5. Salir");
                Console.Write("Seleccione una opción: ");
                string opcion = Console.ReadLine();

                switch (opcion)
                {
                    case "1": RegistrarEstudiante(); break;
                    case "2": EliminarEstudiante(); break;
                    case "3": ConsultarPorMateriaYDia(); break;
                    case "4": ConsultarPorDia(); break;
                    case "5": return;
                    default: Console.WriteLine("Opción inválida."); break;
                }
            }
        }

        static void InicializarAsistencia()
        {
            foreach (string dia in dias)
            {
                asistencia[dia] = new Dictionary<string, List<string>>();
                foreach (string materia in materias)
                {
                    asistencia[dia][materia] = new List<string>();
                }
            }
        }

        static void RegistrarEstudiante()
        {
            string dia = SolicitarDia();
            string materia = SolicitarMateria();
            Console.Write("Ingrese nombre del estudiante: ");
            string nombre = Console.ReadLine();

            var lista = asistencia[dia][materia];

            if (lista.Contains(nombre))
            {
                Console.WriteLine("El estudiante ya está registrado en esta materia y día.");
                return;
            }

            if (lista.Count >= 3)
            {
                Console.WriteLine("No se pueden registrar más de 3 estudiantes por grupo.");
                return;
            }

            lista.Add(nombre);
            Console.WriteLine("Estudiante registrado correctamente.");
        }

        static void EliminarEstudiante()
        {
            string dia = SolicitarDia();
            string materia = SolicitarMateria();
            Console.Write("Ingrese nombre del estudiante a eliminar: ");
            string nombre = Console.ReadLine();

            var lista = asistencia[dia][materia];

            if (lista.Remove(nombre))
            {
                Console.WriteLine("Estudiante eliminado.");
            }
            else
            {
                Console.WriteLine("El estudiante no estaba registrado en esa materia y día.");
            }
        }

        static void ConsultarPorMateriaYDia()
        {
            string dia = SolicitarDia();
            string materia = SolicitarMateria();
            var lista = asistencia[dia][materia];

            Console.WriteLine("\nEstudiantes en " + materia + " el " + dia + ":");
            if (lista.Count == 0)
                Console.WriteLine(" - (ninguno)");
            else
                foreach (var estudiante in lista)
                    Console.WriteLine(" - " + estudiante);
        }

        static void ConsultarPorDia()
        {
            string dia = SolicitarDia();
            Console.WriteLine("\nAsistencia completa del " + dia + ":");

            foreach (var materia in materias)
            {
                Console.WriteLine("  " + materia + ":");
                var lista = asistencia[dia][materia];
                if (lista.Count == 0)
                    Console.WriteLine("   - (ninguno)");
                else
                    foreach (var estudiante in lista)
                        Console.WriteLine("   - " + estudiante);
            }
        }

        static string SolicitarDia()
        {
            while (true)
            {
                Console.Write("Ingrese día (Lunes a Viernes): ");
                string dia = Console.ReadLine();
                foreach (var d in dias)
                {
                    if (string.Equals(dia, d, StringComparison.OrdinalIgnoreCase))
                        return Capitalizar(dia);
                }
                Console.WriteLine("Día inválido.");
            }
        }

        static string SolicitarMateria()
        {
            while (true)
            {
                Console.Write("Ingrese materia: ");
                string materia = Console.ReadLine();
                foreach (var m in materias)
                {
                    if (string.Equals(materia, m, StringComparison.OrdinalIgnoreCase))
                        return Capitalizar(materia);
                }
                Console.WriteLine("Materia inválida.");
            }
        }

        static string Capitalizar(string texto)
        {
            if (string.IsNullOrWhiteSpace(texto)) return "";
            return char.ToUpper(texto[0]) + texto.Substring(1).ToLower();
        }
    }
}
