# Outil-de-gestion-et-d-analyse


import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.util.ArrayList;
import java.util.List;

// Classe représentant un membre de l'équipe
class TeamMember {
    private int id;
    private String name;
    private String role;
    private double performanceScore;

    public TeamMember(int id, String name, String role, double performanceScore) {
        this.id = id;
        this.name = name;
        this.role = role;
        this.performanceScore = performanceScore;
    }

    public String toString() {
        return id + " - " + name + " (" + role + ") : " + performanceScore;
    }
}

public class TeamManagement {
    private static final String URL = "jdbc:postgresql://localhost:5432/team_db";
    private static final String USER = "postgres";
    private static final String PASSWORD = "password";

    public static void main(String[] args) {
        List<TeamMember> members = getAllMembers();
        members.forEach(System.out::println);
    }

    // Récupération des membres depuis la base de données
    public static List<TeamMember> getAllMembers() {
        List<TeamMember> teamMembers = new ArrayList<>();
        String query = "SELECT * FROM team_members";
        
        try (Connection conn = DriverManager.getConnection(URL, USER, PASSWORD);
             PreparedStatement stmt = conn.prepareStatement(query);
             ResultSet rs = stmt.executeQuery()) {
            
            while (rs.next()) {
                teamMembers.add(new TeamMember(
                    rs.getInt("id"),
                    rs.getString("name"),
                    rs.getString("role"),
                    rs.getDouble("performance_score")
                ));
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
        return teamMembers;
    }
}
