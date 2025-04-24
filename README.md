import java.awt.*;
import java.awt.event.*;
import javax.swing.*;

public class MazeGame extends JFrame implements KeyListener {
    final int[][] maze = {
        {1, 1, 1, 1, 1, 1, 1},
        {1, 0, 0, 1, 0, 0, 1},
        {1, 0, 1, 1, 0, 1, 1},
        {1, 0, 0, 0, 0, 0, 1},
        {1, 1, 1, 1, 1, 9, 1}
    };

    final int cellSize = 60;
    int playerX = 1, playerY = 1; // Start position

    public MazeGame() {
        setTitle("Maze Game - Arrow Keys to Play");
        setSize(maze[0].length * cellSize + 40, maze.length * cellSize + 60);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocationRelativeTo(null);
        setVisible(true);
        addKeyListener(this);
        setFocusable(true);
    }

    public void paint(Graphics g) {
        super.paint(g);
        for (int row = 0; row < maze.length; row++) {
            for (int col = 0; col < maze[0].length; col++) {
                switch (maze[row][col]) {
                    case 1:
                        g.setColor(new Color(40, 40, 40)); // Wall - dark gray
                        break;
                    case 0:
                        g.setColor(new Color(255, 255, 200)); // Path - light yellow
                        break;
                    case 9:
                        g.setColor(new Color(255, 50, 50)); // Goal - red
                        break;
                }
                g.fillRect(col * cellSize + 10, row * cellSize + 30, cellSize, cellSize);
                g.setColor(Color.DARK_GRAY);
                g.drawRect(col * cellSize + 10, row * cellSize + 30, cellSize, cellSize);
            }
        }

        // Draw player with blue gradient
        Graphics2D g2d = (Graphics2D) g;
        GradientPaint blueGradient = new GradientPaint(
            playerY * cellSize + 20, playerX * cellSize + 40, new Color(0, 102, 255),
            playerY * cellSize + 40, playerX * cellSize + 60, new Color(0, 153, 255));
        g2d.setPaint(blueGradient);
        g2d.fillOval(playerY * cellSize + 20, playerX * cellSize + 40, cellSize - 20, cellSize - 20);
    }

    void movePlayer(int dx, int dy) {
        int newX = playerX + dx;
        int newY = playerY + dy;
        if (maze[newX][newY] != 1) { // Don't go through walls
            playerX = newX;
            playerY = newY;
            repaint();

            if (maze[playerX][playerY] == 9) {
                JOptionPane.showMessageDialog(this, "Congratulations! You reached the goal!");
            }
        }
    }

    // Keyboard control
    public void keyPressed(KeyEvent e) {
        switch (e.getKeyCode()) {
            case KeyEvent.VK_UP:
                movePlayer(-1, 0);
                break;
            case KeyEvent.VK_DOWN:
                movePlayer(1, 0);
                break;
            case KeyEvent.VK_LEFT:
                movePlayer(0, -1);
                break;
            case KeyEvent.VK_RIGHT:
                movePlayer(0, 1);
                break;
        }
    }

    public void keyReleased(KeyEvent e) {}
    public void keyTyped(KeyEvent e) {}

    public static void main(String[] args) {
        new MazeGame();
    }
}
