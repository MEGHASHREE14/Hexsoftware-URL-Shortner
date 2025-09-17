import java.util.*;

public class URLShortener {
    private static final String BASE = "http://short.ly/";
    private static final String CHARSET = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
    private static final int CODE_LENGTH = 6;

    private Map<String, String> shortToLong = new HashMap<>();
    private Map<String, String> longToShort = new HashMap<>();
    private Random random = new Random();

    // Generate short code
    private String generateCode() {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < CODE_LENGTH; i++) {
            sb.append(CHARSET.charAt(random.nextInt(CHARSET.length())));
        }
        return sb.toString();
    }

    // Shorten URL
    public String shortenURL(String longURL) {
        if (longToShort.containsKey(longURL)) {
            return BASE + longToShort.get(longURL);
        }
        String code;
        do {
            code = generateCode();
        } while (shortToLong.containsKey(code));

        shortToLong.put(code, longURL);
        longToShort.put(longURL, code);

        return BASE + code;
    }

    // Retrieve original URL
    public String getOriginalURL(String shortURL) {
        String code = shortURL.replace(BASE, "");
        return shortToLong.getOrDefault(code, "URL not found");
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        URLShortener shortener = new URLShortener();

        while (true) {
            System.out.println("\n=== URL Shortener ===");
            System.out.println("1. Shorten URL");
            System.out.println("2. Retrieve URL");
            System.out.println("3. Exit");
            System.out.print("Choose: ");
            int choice = sc.nextInt();
            sc.nextLine();

            switch (choice) {
                case 1 -> {
                    System.out.print("Enter long URL: ");
                    String longURL = sc.nextLine();
                    String shortURL = shortener.shortenURL(longURL);
                    System.out.println("Short URL: " + shortURL);
                }
                case 2 -> {
                    System.out.print("Enter short URL: ");
                    String shortURL = sc.nextLine();
                    String longURL = shortener.getOriginalURL(shortURL);
                    System.out.println("Original URL: " + longURL);
                }
                case 3 -> {
                    System.out.println("Goodbye!");
                    sc.close();
                    return;
                }
                default -> System.out.println("Invalid choice!");
            }
        }
    }
}
