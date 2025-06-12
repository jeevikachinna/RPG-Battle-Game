# RPG-Battle-Game  
🔧 Concepts You'll Use:

OOP (Object-Oriented Programming) – Classes, inheritance, polymorphism

Encapsulation – Private fields with public getters/setters

Abstraction – Abstract base class for characters

Collections – ArrayLists for items or enemies

Control Flow – Loops, conditionals for turn-based combat

🎮 Game Features
Two characters: Player and Enemy

Stats: HP, Attack, Defense

Turn-based combat

Simple skills/abilities (e.g., Heavy Attack, Heal)

✅ Next Steps / Extensions
Add inventory and items (e.g. potions)

Multiple enemies and a wave system

Boss battles with special skills

Add experience points and leveling

Implement a GUI using JavaFX later

🧱 Project Structure (Files/Classes)
- Main.java
- Character.java (abstract)
- Player.java
- Enemy.java
- Battle.java
📦 Code Skeleton
Character.java (Abstract Base Class)

public abstract class Character {
    protected String name;
    protected int hp;
    protected int attack;
    protected int defense;

    public Character(String name, int hp, int attack, int defense) {
        this.name = name;
        this.hp = hp;
        this.attack = attack;
        this.defense = defense;
    }

    public boolean isAlive() {
        return hp > 0;
    }

    public void takeDamage(int damage) {
        int actualDamage = damage - defense;
        hp -= Math.max(actualDamage, 0);
        System.out.println(name + " took " + Math.max(actualDamage, 0) + " damage. Remaining HP: " + hp);
    }

    public abstract void attack(Character target);
}
Player.java
import java.util.Scanner;

public class Player extends Character {

    public Player(String name, int hp, int attack, int defense) {
        super(name, hp, attack, defense);
    }

    @Override
    public void attack(Character target) {
        Scanner scanner = new Scanner(System.in);
        System.out.println("Choose an action: 1) Attack  2) Heal");
        int choice = scanner.nextInt();
        if (choice == 1) {
            System.out.println(name + " attacks!");
            target.takeDamage(attack);
        } else if (choice == 2) {
            heal();
        } else {
            System.out.println("Invalid choice. Turn skipped.");
        }
    }

    private void heal() {
        int healAmount = 20;
        hp += healAmount;
        System.out.println(name + " heals for " + healAmount + " HP. Total HP: " + hp);
    }
}
Enemy.java
public class Enemy extends Character {

    public Enemy(String name, int hp, int attack, int defense) {
        super(name, hp, attack, defense);
    }

    @Override
    public void attack(Character target) {
        System.out.println(name + " attacks!");
        target.takeDamage(attack);
    }
}
Battle.java
public class Battle {
    public static void startBattle(Player player, Enemy enemy) {
        while (player.isAlive() && enemy.isAlive()) {
            player.attack(enemy);
            if (!enemy.isAlive()) {
                System.out.println(enemy.name + " has been defeated!");
                break;
            }

            enemy.attack(player);
            if (!player.isAlive()) {
                System.out.println(player.name + " has been defeated!");
            }
        }
    }
}
Main.java
public class Main {
    public static void main(String[] args) {
        Player player = new Player("Knight", 100, 30, 10);
        Enemy enemy = new Enemy("Goblin", 80, 25, 5);

        System.out.println("Battle Start!");
        Battle.startBattle(player, enemy);
    }
}
