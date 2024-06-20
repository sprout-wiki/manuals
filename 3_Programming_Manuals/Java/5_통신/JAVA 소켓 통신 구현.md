
Client.java
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.Socket;

public class Client {
    public static void main(String[] args) {
        final String SERVER_ADDRESS = "localhost"; // 서버 주소
        final int SERVER_PORT = 12345; // 서버 포트 번호
        try {
            // 서버에 연결
            Socket socket = new Socket(SERVER_ADDRESS, SERVER_PORT);
            System.out.println("서버에 연결되었습니다.");

            // 입출력 스트림 생성
            PrintWriter out = new PrintWriter(socket.getOutputStream(), true);
            BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
            BufferedReader stdIn = new BufferedReader(new InputStreamReader(System.in));

            // 사용자로부터 입력을 받아서 서버에 전송
            String userInput;
            while ((userInput = stdIn.readLine()) != null) {
                out.println(userInput);
                System.out.println("서버로 전송한 메시지: " + userInput);
                System.out.println("서버 응답: " + in.readLine());
            }
            // 자원 해제
            out.close();
            in.close();
            stdIn.close();
            socket.close();
        } catch (IOException e) {
            System.err.println("클라이언트 오류: " + e.getMessage());
        }
    }
}
```

Server.java
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.ServerSocket;
import java.net.Socket;

public class Server {
    public static void main(String[] args) {
        final int PORT_NUMBER = 12345; // 포트 번호
        try {
            // 서버 소켓 생성
            ServerSocket serverSocket = new ServerSocket(PORT_NUMBER);
            System.out.println("서버 시작, 클라이언트의 연결을 기다립니다...");
            // 클라이언트 연결을 기다림
            Socket clientSocket = serverSocket.accept();
            System.out.println("클라이언트가 연결되었습니다.");
            // 입출력 스트림 생성
            PrintWriter out = new PrintWriter(clientSocket.getOutputStream(), true);
            BufferedReader in = new BufferedReader(new InputStreamReader(clientSocket.getInputStream()));
            // 클라이언트로부터 메시지 수신 및 송신
            String inputLine;
            while ((inputLine = in.readLine()) != null) {
                System.out.println("클라이언트로부터 수신한 메시지: " + inputLine);
                out.println("서버가 수신한 메시지: " + inputLine);
            }
            // 자원 해제
            in.close();
            out.close();
            clientSocket.close();
            serverSocket.close();
        } catch (IOException e) {
            System.err.println("서버 오류: " + e.getMessage());
        }
    }
}
```