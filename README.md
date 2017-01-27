# exam
/**
 *
 * @author Mar1ach1SD
 */
import java.util.LinkedList;

import java.util.List;

import java.util.PriorityQueue;

import java.util.Queue;

import java.util.Stack;

import java.util.Vector;



public class Grafo1 {

	    char[]  nodos; 

	    double[][] grafo;  // tu cora del mio
            

	    String  rutaMasCorta;                           // pequeñin 1

	    int     longitudMasCorta = Integer.MAX_VALUE;   // pequeñin2

	     List<Nodo>  listos=null;                        

	 

	    

	    Grafo1(String serieNodos) {

	        nodos = serieNodos.toCharArray();

	        grafo = new double[nodos.length][nodos.length];

	    }


	    public void agregarRuta(char origen, char destino, double d) {

	        int n1 = posicionNodo(origen);

	        int n2 = posicionNodo(destino);

	        grafo[n1][n2]=d;

	        grafo[n2][n1]=d;
	      
	     
	     System.out.println(grafo[n1][n2]);
	        
	    }

	 

	    

	    private int posicionNodo(char nodo) {

	        for(int i=0; i<nodos.length; i++) {

	            if(nodos[i]==nodo) return i;

	        }

	        return -1;

	    }

	     

	//Buscando a nemo

	    public String encontrarRutaMinimaDijkstra(char inicio, char fin) {

	        // ELpequeñin

	        encontrarRutaMinimaDijkstra(inicio);

	        

	        Nodo tmp = new Nodo(fin);

	        if(!listos.contains(tmp)) {

	            System.out.println("Error, nodo no alcanzable");

	            return "Bye";

	        }

	        tmp = listos.get(listos.indexOf(tmp));

	        double distancia = tmp.distancia;  

	        // Almacen

	        Stack<Nodo> pila = new Stack<Nodo>();

	        while(tmp != null) {

	            pila.add(tmp);

	            tmp = tmp.procedencia;

	        }

	        String ruta = "";

	        // recorrido

	        while(!pila.isEmpty()) ruta+=(pila.pop().id + " ");

	        return distancia + ": " + ruta;

	    }

	 

	    // Buscando a nemo

	    public void encontrarRutaMinimaDijkstra(char inicio) {

	        Queue<Nodo>   cola = new PriorityQueue<Nodo>(); // cola de prioridad

	        Nodo            ni = new Nodo(inicio);          // nodo inicial

	         

	        listos = new LinkedList<Nodo>();

	        cola.add(ni);                  

	        while(!cola.isEmpty()) {        

	            Nodo tmp = cola.poll();    

	            listos.add(tmp);            

	            int p = posicionNodo(tmp.id);   

	            for(int j=0; j<grafo[p].length; j++) {  

	                if(grafo[p][j]==0) continue;        

	                if(estaTerminado(j)) continue;     

	                Nodo nod = new Nodo(nodos[j],tmp.distancia+grafo[p][j],tmp);

	               

	                if(!cola.contains(nod)) {

	                    cola.add(nod);

	                    continue;

	                }

	               

	                for(Nodo x: cola) {

	                   

	                    if(x.id==nod.id && x.distancia > nod.distancia) {

	                        cola.remove(x); 

	                        cola.add(nod);

	                        break;          

	                    }

	                }

	            }

	        }

	    }
	  

	    public boolean estaTerminado(int j) {

	        Nodo tmp = new Nodo(nodos[j]);

	        return listos.contains(tmp);

            }

	    //  ruta mínima mamada

	    public void encontrarRutaMinimaFuerzaBruta(char inicio, char fin) {

	        int p1 = posicionNodo(inicio);

	        int p2 = posicionNodo(fin);

	        // cola 

	        Stack<Integer> resultado = new Stack<Integer>();

	        resultado.push(p1);

	        recorrerRutas(p1, p2, resultado);

	    }

	 

	    // recursivamente

	    private void recorrerRutas(int nodoI, int nodoF, Stack<Integer> resultado){

	        if(nodoI==nodoF) {
int respuesta1= evaluar(resultado);
	            int respuesta = evaluar(resultado);

	            if(respuesta < longitudMasCorta) {

	                longitudMasCorta = respuesta;

	                rutaMasCorta     = "";

	                for(int x: resultado) rutaMasCorta+=(nodos[x]+" ");

	            }

	            return;

	        }

	        

	        List<Integer> lista = new Vector<Integer>();

	        for(int i=0; i<grafo.length;i++) {

	            if(grafo[nodoI][i]!=0 && !resultado.contains(i))lista.add(i);

	        }

	        for(int nodo: lista) {

	            resultado.push(nodo);

	            recorrerRutas(nodo, nodoF, resultado);

	            resultado.pop();

	        }

	    }

	 

	    

	    public int evaluar(Stack<Integer> resultado) {

	        int  resp = 0;
                int resp2 = 0;

	        int[]   r = new int[resultado.size()];

                 int[]   re = new int[resultado.size()];
	        int     i = 0;

	        for(int x: resultado) r[i++]=x;

	        for(i=1; i<r.length; i++) resp+=grafo[r[i]][r[i-1]];
	        System.out.println(resp+=grafo[r[i]][r[i-1]]);
	        return resp;
                 
                
	    

            }

	    public static void main(String[] args) {
	        Grafo1 g = new Grafo1("abcdef");

System.out.println("De plaza Romblas a alameda");
	        g.agregarRuta('a','b', 1.6);
System.out.println("De plaza Alameda a Club plaza");
	        g.agregarRuta('b','d', 1.7);
System.out.println("De plaza Alameda a Patria");
	        g.agregarRuta('b','c',1.2);
System.out.println("De plaza patria a Plaza mexico");
	        g.agregarRuta('c','e', 4.3);
System.out.println("De plaza Club plaza a Plaza mexico");
	        g.agregarRuta('d','e', 2.8);
System.out.println("De plaza Galerias a Plaza mexico");
	        g.agregarRuta('f','e', 18);
System.out.println("De plaza Romblas a Galerias");
	        g.agregarRuta('a','f', 9.2);

	    

	        char inicio1 = 'b';

	        char fin1    = 'd';
	        

	        String respuesta = g.encontrarRutaMinimaDijkstra(inicio1, fin1);
                
	        
	        

	     
	        System.out.println("Menu de plazas \na Romblas \nb Alameda \nc Patria \nd Club plaza \ne Plaza mexico \nf Galerias");
	        System.out.println("\nRuta mas corta de ramblaz a Plaza mexico(tomando encuenta plazas como referencia)\n"+respuesta);
             
                
	        
	       

	    

}
        

//Nodo

/**
 *
 * @author Mar1ach1SD
 */
public class Nodo implements Comparable<Nodo> {

	    char id;

	    double  distancia   = Integer.MAX_VALUE;

	    Nodo procedencia = null;

	    Nodo(char x, double d, Nodo p) { id=x; distancia=d; procedencia=p; }

	    Nodo(char x) { this(x, 0, null); }

	    public int compareTo(Nodo tmp) { return (int) (this.distancia-tmp.distancia); }

	    public boolean equals(Object o) {

	        Nodo tmp = (Nodo) o;

	        if(tmp.id==this.id) return true;

	        return false;

	    }

	}
