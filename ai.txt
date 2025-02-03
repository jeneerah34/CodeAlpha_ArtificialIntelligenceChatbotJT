import java.util.Scanner;
import java.util.HashMap;
import java.io.File;
import java.io.FileWriter;
import java.io.IOException;

public class AIChatbot {

    private static HashMap<String, String> memory = new HashMap<>();
    private static boolean isRunning = true;

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.println("AI Chatbot: Hello! I am your AI chatbot. Type 'exit' to end the chat.");
        
        while (isRunning) {
            System.out.print("You: ");
            String userInput = scanner.nextLine();
            
            if (userInput.equalsIgnoreCase("exit")) {
                System.out.println("AI Chatbot: Goodbye!");
                isRunning = false;
                break;
            }

            String response = generateResponse(userInput);
            System.out.println("AI Chatbot: " + response);
            saveConversation(userInput, response);
        }

        scanner.close();
    }

    private static String generateResponse(String userInput) {
        userInput = userInput.toLowerCase();
        
        // Memory-based response
        if (memory.containsKey(userInput)) {
            return memory.get(userInput);
        }
        
        // Basic responses
        if (userInput.contains("hello")) {
            return "Hello! How can I assist you today?";
        } else if (userInput.contains("how are you")) {
            return "I'm just a bot, but I'm doing great! How about you?";
        } else if (userInput.contains("bye")) {
            return "Goodbye! Have a great day!";
        } else {
            return "I'm still learning! Can you rephrase that?";
        }
    }

    private static void saveConversation(String userInput, String response) {
        try {
            File file = new File("conversation_log.txt");
            if (!file.exists()) {
                file.createNewFile();
            }
            FileWriter writer = new FileWriter(file, true);
            writer.write("User: " + userInput + "\n");
            writer.write("AI Chatbot: " + response + "\n\n");
            writer.close();
        } catch (IOException e) {
            System.out.println("Error saving conversation.");
        }
    }
}