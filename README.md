# java-complaint-classifier
java-complaint-classifier......
import java.util.*;

public class ComplaintClassifier {

    static Map<String, List<String>> categoryKeywords = new LinkedHashMap<>();

    static void initCategories() {
        categoryKeywords.put("Hostel", Arrays.asList("hostel", "room", "warden", "mess", "water", "bed"));
        categoryKeywords.put("Academic", Arrays.asList("exam", "marks", "teacher", "syllabus", "class", "result"));
        categoryKeywords.put("Infrastructure", Arrays.asList("wifi", "internet", "lab", "projector", "electricity", "fan", "ac"));
        categoryKeywords.put("Transport", Arrays.asList("bus", "route", "driver", "late", "transport"));
        categoryKeywords.put("Administration", Arrays.asList("fee", "certificate", "form", "office", "staff", "document"));
    }

    static String classify(String complaint) {
        String text = complaint.toLowerCase();
        String bestCategory = "General";
        int bestScore = 0;

        for (Map.Entry<String, List<String>> entry : categoryKeywords.entrySet()) {
            int score = 0;
            for (String keyword : entry.getValue()) {
                if (text.contains(keyword)) score++;
            }
            if (score > bestScore) {
                bestScore = score;
                bestCategory = entry.getKey();
            }
        }
        return bestCategory;
    }

    static String priority(String complaint) {
        String text = complaint.toLowerCase();
        if (text.contains("urgent") || text.contains("danger") || text.contains("not working for days"))
            return "HIGH";
        return "NORMAL";
    }

    public static void main(String[] args) {
        initCategories();

        String[] complaints = {
            "The wifi in the lab is not working for days",
            "Hostel mess food quality is very poor and water is dirty",
            "Bus driver is always late on the Ranchi route",
            "My exam marks are not updated in the result portal",
            "Fee receipt certificate not issued by office, urgent"
        };

        System.out.println("=== Complaint Classification Report ===\n");
        for (String c : complaints) {
            System.out.printf("Complaint : %s%nCategory  : %s%nPriority  : %s%n%n",
                    c, classify(c), priority(c));
        }
    }
}
