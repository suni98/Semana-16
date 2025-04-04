# Semana-16
using System;
using System.Collections.Generic;
using System.Linq;

// Clase para representar un nodo del grafo
public class Nodo
{
    public string Nombre { get; set; }
    public List<Nodo> Vecinos { get; set; } = new List<Nodo>();

    public Nodo(string nombre)
    {
        Nombre = nombre;
    }
}

// Clase para representar el grafo
public class Grafo
{
    public Dictionary<string, Nodo> Nodos { get; set; } = new Dictionary<string, Nodo>();

    public void AgregarNodo(string nombre)
    {
        Nodos[nombre] = new Nodo(nombre);
    }

    public void AgregarArista(string origen, string destino)
    {
        if (Nodos.ContainsKey(origen) && Nodos.ContainsKey(destino))
        {
            Nodos[origen].Vecinos.Add(Nodos[destino]);
            Nodos[destino].Vecinos.Add(Nodos[origen]); // Si el grafo es no dirigido
        }
    }

    // Recorrido en Profundidad (DFS)
    public List<string> DFS(string inicio)
    {
        List<string> recorrido = new List<string>();
        HashSet<string> visitados = new HashSet<string>();
        DFSUtil(inicio, visitados, recorrido);
        return recorrido;
    }

    private void DFSUtil(string nodoActual, HashSet<string> visitados, List<string> recorrido)
    {
        visitados.Add(nodoActual);
        recorrido.Add(nodoActual);

        foreach (var vecino in Nodos[nodoActual].Vecinos)
        {
            if (!visitados.Contains(vecino.Nombre))
            {
                DFSUtil(vecino.Nombre, visitados, recorrido);
            }
        }
    }

    // Recorrido en Anchura (BFS)
    public List<string> BFS(string inicio)
    {
        List<string> recorrido = new List<string>();
        Queue<string> cola = new Queue<string>();
        HashSet<string> visitados = new HashSet<string>();

        cola.Enqueue(inicio);
        visitados.Add(inicio);

        while (cola.Count > 0)
        {
            string nodoActual = cola.Dequeue();
            recorrido.Add(nodoActual);

            foreach (var vecino in Nodos[nodoActual].Vecinos)
            {
                if (!visitados.Contains(vecino.Nombre))
                {
                    cola.Enqueue(vecino.Nombre);
                    visitados.Add(vecino.Nombre);
                }
            }
        }
        return recorrido;
    }
}

public class Program
{
    public static void Main(string[] args)
    {
        // Crear un grafo de ejemplo
        Grafo grafo = new Grafo();
        grafo.AgregarNodo("A");
        grafo.AgregarNodo("B");
        grafo.AgregarNodo("C");
        grafo.AgregarNodo("D");
        grafo.AgregarNodo("E");

        grafo.AgregarArista("A", "B");
        grafo.AgregarArista("A", "C");
        grafo.AgregarArista("B", "D");
        grafo.AgregarArista("C", "E");
        grafo.AgregarArista("D", "E");

        // Realizar recorridos
        Console.WriteLine("Recorrido DFS desde A: " + string.Join(", ", grafo.DFS("A")));
        Console.WriteLine("Recorrido BFS desde A: " + string.Join(", ", grafo.BFS("A")));
    }
}
