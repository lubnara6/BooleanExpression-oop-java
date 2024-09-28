import javax.swing.*;
import javax.swing.border.LineBorder;
import javax.swing.border.TitledBorder;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class CombinedGUI extends JFrame {

    private JTextField[] variableFields;
    private JLabel[] expressionLabels;
    private JLabel[] resultLabels;

    public CombinedGUI() {
        setTitle("Combined GUI");
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setPreferredSize(new Dimension(1200, 600));
        getContentPane().setBackground(new Color(0xF2F2F2)); // Light Gray
        setLayout(new BorderLayout());

        // Main Panel
        JPanel mainPanel = new JPanel();
        mainPanel.setLayout(new BorderLayout());
        mainPanel.setBackground(new Color(0xF2F2F2)); // Light Gray
        mainPanel.setBorder(BorderFactory.createEmptyBorder(20, 20, 20, 20));

        // Header Panel
        JPanel headerPanel = new JPanel();
        headerPanel.setBackground(new Color(0x4CAF50)); // Green
        JLabel headerLabel = new JLabel("Boolean Expression Evaluator");
        headerLabel.setForeground(Color.WHITE);
        headerLabel.setFont(new Font("Arial", Font.BOLD, 28));
        headerPanel.add(headerLabel);
        mainPanel.add(headerPanel, BorderLayout.NORTH);

        // Input Panel
        JPanel inputPanel = new JPanel();
        inputPanel.setBackground(new Color(0xFFFFFF)); // White
        inputPanel.setBorder(BorderFactory.createTitledBorder(new LineBorder(Color.BLACK, 2), "Enter Variables", TitledBorder.DEFAULT_JUSTIFICATION, TitledBorder.DEFAULT_POSITION, new Font("Arial", Font.BOLD, 18), Color.BLACK));
        GroupLayout layout = new GroupLayout(inputPanel);
        inputPanel.setLayout(layout);
        layout.setAutoCreateGaps(true);
        layout.setAutoCreateContainerGaps(true);

        variableFields = new JTextField[6];
        GroupLayout.SequentialGroup hGroup = layout.createSequentialGroup();
        GroupLayout.SequentialGroup vGroup = layout.createSequentialGroup();

        for (int i = 0; i < variableFields.length; i++) {
            JLabel label = new JLabel(Character.toString((char) ('p' + i)));
            label.setFont(new Font("Arial", Font.BOLD, 16));
            JTextField textField = new JTextField(10);
            textField.setFont(new Font("Arial", Font.PLAIN, 16));
            variableFields[i] = textField;
            hGroup.addGroup(layout.createParallelGroup(GroupLayout.Alignment.LEADING)
                    .addComponent(label)
                    .addComponent(textField));
            vGroup.addGroup(layout.createParallelGroup(GroupLayout.Alignment.BASELINE)
                    .addComponent(label)
                    .addComponent(textField));
        }

        layout.setHorizontalGroup(hGroup);
        layout.setVerticalGroup(vGroup);

        mainPanel.add(inputPanel, BorderLayout.WEST);

        // Expression Panel
        JPanel expressionPanel = new JPanel();
        expressionPanel.setBackground(new Color(0xFFFFFF)); // White
        expressionPanel.setBorder(BorderFactory.createTitledBorder(new LineBorder(Color.BLACK, 2), "Boolean Expressions", TitledBorder.DEFAULT_JUSTIFICATION, TitledBorder.DEFAULT_POSITION, new Font("Arial", Font.BOLD, 18), Color.BLACK));
        expressionPanel.setLayout(new GridLayout(0, 1));

        expressionLabels = new JLabel[]{
                new JLabel("p OR q"),          // For p OR q
                new JLabel("p XOR q"),         // For p XOR q
                new JLabel("(p XOR q) OR p")   // For (p XOR q) OR p
                // Add more expressions as needed
        };

        for (JLabel label : expressionLabels) {
            label.setFont(new Font("Arial", Font.BOLD, 16));
            expressionPanel.add(label);
        }

        resultLabels = new JLabel[expressionLabels.length];
        for (int i = 0; i < expressionLabels.length; i++) {
            resultLabels[i] = new JLabel("Result: ");
            resultLabels[i].setFont(new Font("Arial", Font.PLAIN, 16));
            expressionPanel.add(resultLabels[i]);
        }

        mainPanel.add(expressionPanel, BorderLayout.CENTER);

        // Button Panel
        JPanel buttonPanel = new JPanel(new FlowLayout(FlowLayout.CENTER));
        buttonPanel.setBackground(new Color(0xF2F2F2)); // Light Gray
        JButton evaluateButton = new JButton("Evaluate");
        evaluateButton.setFont(new Font("Arial", Font.BOLD, 18));
        evaluateButton.setBackground(new Color(0x4CAF50)); // Green
        evaluateButton.setForeground(Color.WHITE);
        evaluateButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                boolean allVariablesEntered = true;
                for (int i = 0; i < variableFields.length; i++) {
                    if (variableFields[i].getText().trim().isEmpty()) {
                        allVariablesEntered = false;
                        break;
                    }
                }

                if (!allVariablesEntered) {
                    JOptionPane.showMessageDialog(null, "Please enter all variables.", "Missing Variables", JOptionPane.WARNING_MESSAGE);
                    return;
                }

                // Proceed with evaluation if all variables are entered
                boolean[] variables = new boolean[variableFields.length];
                for (int i = 0; i < variableFields.length; i++) {
                    variables[i] = parseBoolean(variableFields[i].getText().trim());
                }

                // Evaluation logic for each expression
                for (int i = 0; i < resultLabels.length; i++) {
                    resultLabels[i].setText("Result: " + evaluateExpression(i, variables));
                }
            }

            private boolean evaluateExpression(int index, boolean[] variables) {
                switch (index) {
                    case 0:
                        return variables[0] || variables[1];
                    case 1:
                        return variables[0] ^ variables[1];
                    case 2:
                        return (variables[0] ^ variables[1]) || variables[0];
                    case 3:
                        return (variables[0] && variables[1]) || (variables[2] && variables[3]);
                    default:
                        return false;
                }
            }
        });
        buttonPanel.add(evaluateButton);
        mainPanel.add(buttonPanel, BorderLayout.SOUTH);

        add(mainPanel, BorderLayout.CENTER);

        // Text GUI Panel
        JPanel textPanel = new JPanel();
        textPanel.setLayout(new BorderLayout());
        textPanel.setBackground(new Color(0xFFFFFF)); // White
        textPanel.setBorder(BorderFactory.createEmptyBorder(20, 20, 20, 20));

        JLabel welcomeLabel = new JLabel("Welcome to Loubna Text GUI");
        welcomeLabel.setHorizontalAlignment(JLabel.CENTER);
        welcomeLabel.setFont(new Font("Arial", Font.BOLD, 24));
        textPanel.add(welcomeLabel, BorderLayout.CENTER);

        JLabel copyrightLabel = new JLabel("\u00A9 2024 All Rights Reserved - Loubna");
        copyrightLabel.setHorizontalAlignment(JLabel.CENTER);
        textPanel.add(copyrightLabel, BorderLayout.SOUTH);

        add(textPanel, BorderLayout.EAST);

        pack();
        setLocationRelativeTo(null);
    }

    private boolean parseBoolean(String input) {
        return input.equals("1") || input.equalsIgnoreCase("true");
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            try {
                UIManager.setLookAndFeel(UIManager.getSystemLookAndFeelClassName());
            } catch (Exception e) {
                e.printStackTrace();
            }
            CombinedGUI combinedGUI = new CombinedGUI();
            combinedGUI.setVisible(true);
        });
    }
}
