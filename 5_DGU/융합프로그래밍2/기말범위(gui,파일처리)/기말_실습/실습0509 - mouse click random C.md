```java
package silsub0509_randomC;

import javax.swing.*;
import java.awt.*;
import java.awt.event.MouseAdapter;
import java.awt.event.MouseEvent; 	
import java.util.Random;

public class randomC {
    public static void main(String[] args) {
        SwingUtilities.invokeLater(randomC::createAndShowGUI);
    }

    private static void createAndShowGUI() {
        JFrame frame = new JFrame("Mouse Click Random Position");
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setSize(400, 300);
        frame.setLayout(null);

        JLabel label = new JLabel("c");
        label.setFont(new Font("Arial", Font.PLAIN, 15));
        frame.getContentPane().add(label);

        label.setSize(label.getPreferredSize());

        label.addMouseListener(new MouseAdapter() {
            @Override
            public void mouseClicked(MouseEvent e) {
                Random random = new Random();
                int contentPaneWidth = label.getParent().getWidth();
                int contentPaneHeight = label.getParent().getHeight();
                int newX = random.nextInt(contentPaneWidth - label.getWidth());
                int newY = random.nextInt(contentPaneHeight - label.getHeight());
                label.setLocation(newX, newY);
            }
        });

        Random random = new Random();
        int initialX = random.nextInt(frame.getWidth() - label.getWidth()*2);
        int initialY = random.nextInt(frame.getHeight() - label.getHeight()*2);
        label.setLocation(initialX, initialY);

        frame.setVisible(true);
    }
}



```