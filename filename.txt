import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class Calculator extends JFrame {

    // Declare the components
    private JTextField display;
    private JButton[] numberButtons;
    private JButton[] operationButtons;
    private JButton clearButton, equalButton, dotButton;
    private String operator = "";
    private double firstNum = 0;
    private boolean isOperatorClicked = false;

    // Constructor to set up the GUI
    public Calculator() {
        setTitle("Calculator");
        setSize(400, 500);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocationRelativeTo(null);

        // Create the display field
        display = new JTextField();
        display.setFont(new Font("Arial", Font.PLAIN, 24));
        display.setEditable(false);
        display.setHorizontalAlignment(SwingConstants.RIGHT);

        // Create number buttons (0-9) and operation buttons (+, -, *, /)
        numberButtons = new JButton[10];
        for (int i = 0; i < 10; i++) {
            numberButtons[i] = new JButton(String.valueOf(i));
        }

        operationButtons = new JButton[4];
        String[] operations = {"+", "-", "*", "/"};
        for (int i = 0; i < 4; i++) {
            operationButtons[i] = new JButton(operations[i]);
        }

        // Create other buttons
        clearButton = new JButton("C");
        equalButton = new JButton("=");
        dotButton = new JButton(".");

        // Create the panel and set layout
        JPanel panel = new JPanel();
        panel.setLayout(new GridLayout(5, 4, 10, 10));

        // Add the components to the panel
        panel.add(display);
        panel.add(clearButton);
        panel.add(dotButton);
        panel.add(equalButton);

        for (int i = 1; i <= 9; i++) {
            panel.add(numberButtons[i]);
        }
        panel.add(numberButtons[0]);

        for (JButton button : operationButtons) {
            panel.add(button);
        }

        // Add action listeners to the buttons
        for (int i = 0; i < 10; i++) {
            numberButtons[i].addActionListener(new ActionListener() {
                public void actionPerformed(ActionEvent e) {
                    if (isOperatorClicked) {
                        display.setText("");
                        isOperatorClicked = false;
                    }
                    display.setText(display.getText() + e.getActionCommand());
                }
            });
        }

        // Add action listeners for operation buttons
        for (JButton button : operationButtons) {
            button.addActionListener(new ActionListener() {
                public void actionPerformed(ActionEvent e) {
                    operator = e.getActionCommand();
                    firstNum = Double.parseDouble(display.getText());
                    isOperatorClicked = true;
                }
            });
        }

        // Action listener for the equals button
        equalButton.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                double secondNum = Double.parseDouble(display.getText());
                switch (operator) {
                    case "+":
                        display.setText(String.valueOf(firstNum + secondNum));
                        break;
                    case "-":
                        display.setText(String.valueOf(firstNum - secondNum));
                        break;
                    case "*":
                        display.setText(String.valueOf(firstNum * secondNum));
                        break;
                    case "/":
                        if (secondNum != 0) {
                            display.setText(String.valueOf(firstNum / secondNum));
                        } else {
                            display.setText("Error");
                        }
                        break;
                }
                operator = "";
                firstNum = 0;
            }
        });

        // Action listener for the clear button
        clearButton.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                display.setText("");
                firstNum = 0;
                operator = "";
            }
        });

        // Action listener for the dot button (for decimal)
        dotButton.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                if (!display.getText().contains(".")) {
                    display.setText(display.getText() + ".");
                }
            }
        });

        // Add the panel to the frame
        add(panel);

        setVisible(true);
    }

    public static void main(String[] args) {
        new Calculator(); // Launch the calculator
    }
}
