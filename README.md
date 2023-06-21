# SocketApp
# ServidorSocket:

import java.io.*;
import java.net.*;

public class ServidorSocket2 {
    public static void main(String[] args) {
        ServerSocket ss = null;
        Socket s = null;
        BufferedReader textoRecibidoDelCliente;
        DataOutputStream textoEnviarAlCliente;

        try {
            ss = new ServerSocket(5432);

            System.out.println("Servidor escuchando");
            s = ss.accept();
            System.out.println("Cliente conectado");
            textoRecibidoDelCliente = new BufferedReader(new InputStreamReader(s.getInputStream()));
            textoEnviarAlCliente = new DataOutputStream(s.getOutputStream());

            int mayor = Integer.MIN_VALUE;
            int menor = Integer.MAX_VALUE;
            int suma = 0;

            for (int i = 0; i < 10; i++) {
                String textoRecibido = textoRecibidoDelCliente.readLine();
                int numero = Integer.parseInt(textoRecibido);
                mayor = Math.max(mayor, numero);
                menor = Math.min(menor, numero);
                suma += numero;
            }

            String respuesta = "El número mayor es " + mayor + ", el número menor es " + menor
                    + ", y la suma de todos los números digitados es " + suma;
            textoEnviarAlCliente.writeUTF(respuesta);

            textoRecibidoDelCliente.close();
            textoEnviarAlCliente.close();
            s.close();
            ss.close();
        } catch (IOException e) {
            System.err.println(e.getMessage());
            System.exit(1);
        }
    }
}
//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
#ClienteSocket2:
import java.io.*;
import java.net.*;

public class ClienteSocket2 {
    public static void main(String[] args) throws Exception {
        Socket s = null;
        BufferedReader textoDelTeclado;
        DataOutputStream textoAlServidor;
        DataInputStream textoDelServidor;

        try {
            s = new Socket("127.0.0.1", 5432);
            System.out.println("Conexión establecida con el servidor");
            textoDelTeclado = new BufferedReader(new InputStreamReader(System.in));
            textoAlServidor = new DataOutputStream(s.getOutputStream());
            textoDelServidor = new DataInputStream(s.getInputStream());

            for (int i = 0; i < 10; i++) {
                System.out.print("Ingresa el número " + (i + 1) + ": ");
                String numero = textoDelTeclado.readLine();
                textoAlServidor.writeUTF(numero);
            }

            String respuesta = textoDelServidor.readUTF();
            System.out.println(respuesta);

            textoDelServidor.close();
            textoDelTeclado.close();
            textoAlServidor.close();
            s.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}


