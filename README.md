import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import javax.swing.event.*;
/**
 * Calculator for basic calculation functions +, -, 8, / and intiger. 
 * created using resizable GUI interface. 
 * Calculator contains 23 buttons and display pannel
 *
 * @author Rasa
 * @Date 2022 01 30
   */
public class SimpleCalc extends JFrame implements ActionListener {
    JTextField display; //calculator's display

    // initialise instance variables
    JButton[] digitButtons; 
    JButton[] operatorButtons;
    JButton[] memoryButtons;
    JButton AnswerButton;
    JButton intigerButton;
    JButton offButton;
    
    double temp, temp1, result, a;
    static double m1, m2;
    int k = 1, x = 0, y = 0, z = 0;
    char ch;
    int inputInteger = 0;

    //button text    
    String digitButton[] = {"1", "2", "3", "4", "5", "6", "7", "8", "9", "+/-", "0", "." };  
    String operatorButton[] = {"+",  "-", "*", "/"};  
    String memoryButton[] = {"<<", "C", "(", ")"};  
    
    //button commands
    String[] digitButtonCommands = {"CMD_1","CMD_2","CMD_3","CMD_4","CMD_5",
        "CMD_6","CMD_7","CMD_8","CMD_9","CMD_PLUSMINUS", "CMD_0","CMD_DOT"};
    String[] operatorButtonCommands = {"CMD_ADD","CMD_MIN","CMD_MUL","CMD_DIV"};
    String[] memoryButtonCommands = {"CMD_DEL","CMD_C","CMD_LEFT","CMD_RIGHT"};
    
          
    //Constructor for objects of class SimpleCalc
    public SimpleCalc(){
        super("Calculator By Rasa");
        setSize(400,450);
        setMinimumSize(new Dimension(470,400));
        CreateCalcGUI();
    }

    //A method to set up the GUI
    //creating 4 panels
    private void CreateCalcGUI() {
        int eb = 10;// outer border for pannels

        // panelTop will be used for display
        JPanel panelTop = new JPanel();
        panelTop.setBorder(BorderFactory.createCompoundBorder(
         BorderFactory.createEmptyBorder(eb, eb, eb, eb), // outer border  (top, left, bottom, rigft)
         BorderFactory.createLoweredBevelBorder()));      // inner border
        panelTop.setBackground(Color.white); //pannels colour
        GridLayout panelTopLayout = new GridLayout(0,1); //number of objects in one row of column
        panelTop.setLayout(panelTopLayout);
     
        // panelWest will be used for numbers
        JPanel panelWest = new JPanel();
        panelWest.setPreferredSize(new Dimension(200, 80));
        panelWest.setBackground(Color.white);
         panelWest.setBorder(BorderFactory.createCompoundBorder(
         BorderFactory.createEmptyBorder(eb, eb, eb, eb), 
         BorderFactory.createLoweredBevelBorder()));      
        GridLayout panelWestLayout = new GridLayout(5,3);
        panelWestLayout.setHgap(10);
        panelWestLayout.setVgap(10);
        panelWest.setLayout(panelWestLayout);
     
        // panelCentre will be used for operator buttons
        JPanel panelCenter = new JPanel();
        panelCenter.setBorder(BorderFactory.createCompoundBorder(
          BorderFactory.createEmptyBorder(eb, eb, eb, eb),
          BorderFactory.createLoweredBevelBorder())); 
        
        panelCenter.setBackground(Color.white);        
        GridLayout panelCenterLayout = new GridLayout(5,1);
        panelCenterLayout.setHgap(10);
        panelCenterLayout.setVgap(10);
        panelCenter.setLayout(panelCenterLayout);    
        
        // panelTop will be used for memory buttons
        JPanel panelEast = new JPanel();
         panelEast.setBorder(BorderFactory.createCompoundBorder(
         BorderFactory.createEmptyBorder(eb, eb, eb, eb), 
         BorderFactory.createLoweredBevelBorder())); 
        panelEast.setPreferredSize(new Dimension(90, 80));
        panelEast.setBackground(Color.white);        
        GridLayout panelEastLayout = new GridLayout(5,1);
        panelEastLayout.setHgap(10);
        panelEastLayout.setVgap(10);
        panelEast.setLayout(panelEastLayout);    
        
        //using border layout system, putting all panels in logical order
        BorderLayout mainLayout = new BorderLayout();
        setLayout(mainLayout);
        add(panelTop, BorderLayout.NORTH);
        add(panelWest, BorderLayout.WEST);
        add(panelCenter, BorderLayout.CENTER);
        add(panelEast, BorderLayout.EAST);
        
        //dispaly look
        display = new JTextField("");        
        Font displayFont = new Font("SansSerif", Font.BOLD, 24);
    display.setFont(displayFont);
    display.setHorizontalAlignment(JTextField.RIGHT);        
        display.setPreferredSize(new Dimension(100,35));
        panelTop.add(display);
        
        //creating digit buttons
        digitButtons = new JButton[12];
        for (int i = 0; i < digitButtons.length; i++) {
            digitButtons[i] = new JButton();
            Color myNewOrange = new Color (255, 229, 229);
            digitButtons[i].setBackground(myNewOrange);
            digitButtons[i].setText(digitButton[i]);
            digitButtons[i].setActionCommand(digitButtonCommands[i]);
            digitButtons[i].addActionListener(this);
            panelWest.add(digitButtons[i]);
        }
        
        //creating operator buttons
        operatorButtons = new JButton[4];
        for (int i = 0; i < operatorButtons.length; i++) {
            operatorButtons[i] = new JButton();
            Color myNewOrange = new Color (255, 229, 229);
            operatorButtons[i].setBackground(myNewOrange);
            operatorButtons[i].setText(operatorButton[i]);
            operatorButtons[i].setActionCommand(operatorButtonCommands[i]);
            operatorButtons[i].addActionListener(this);
            panelCenter.add(operatorButtons[i]);
        }
        
        //creating memory buttons
        memoryButtons = new JButton[4];
        for (int i = 0; i < memoryButtons.length; i++) {
            memoryButtons[i] = new JButton();
            Color myNewOrange = new Color (255, 229, 229);
            memoryButtons[i].setBackground(myNewOrange);
            memoryButtons[i].setText(memoryButton[i]);
            memoryButtons[i].setActionCommand(memoryButtonCommands[i]);
            memoryButtons[i].addActionListener(this);
            panelEast.add(memoryButtons[i]);
        }
    
       //few buttons created outside of the panel to have an option to change 
       //button setting separately
        Color myNewOrange = new Color (255, 229, 229);  //creates your new color
        Color myNewPink = new Color (255, 204, 204);
        AnswerButton = new JButton("=");
        AnswerButton.setBackground(myNewPink);
        AnswerButton.addActionListener(this);
        AnswerButton.setPreferredSize(new Dimension(100, 60));
        AnswerButton.setActionCommand("CMD_ANSW");
        
        intigerButton = new JButton("!");
        intigerButton.setBackground(myNewOrange);
        intigerButton.setPreferredSize(new Dimension(10, 60));
        intigerButton.addActionListener(this);
        intigerButton.setActionCommand("CMD_INT");
        
        offButton = new JButton("OFF");
        offButton.setBackground(myNewPink);
        offButton.setPreferredSize(new Dimension(10, 60));
        offButton.addActionListener(this);
        offButton.setActionCommand("CMD_OFF");
        
        //adding last 3 buttong to pannels
        panelWest.add(AnswerButton);
        panelCenter.add(intigerButton);
        panelEast.add(offButton);        
        
    }

    
    //actionPerformed method creates presses button command and assigning value to it
    //also creating all the basic calculation
       public void actionPerformed(ActionEvent e) {
        String displayText;
        displayText = display.getText();
        String command = e.getActionCommand();
        
        if (command.equals("CMD_1")) {
          display.setText(display.getText() + "1");
        } else
        if (command.equals("CMD_2")) {
           display.setText(display.getText() + "2");
        } else
        if (command.equals("CMD_3")) {
           display.setText(display.getText() + "3");
        } else
        if (command.equals("CMD_4")) {
           display.setText(display.getText() + "4");
        } else
        if (command.equals("CMD_5")) {
          display.setText(display.getText() + "5");
        } else
        if (command.equals("CMD_6")) {
           display.setText(display.getText() + "6");
        } else
        if (command.equals("CMD_7")) {
           display.setText(display.getText() + "7");
        } else
        if (command.equals("CMD_8")) {
           display.setText(display.getText() + "8");
        } else
        if (command.equals("CMD_9")) {
           display.setText(display.getText() + "9");
        } else
        if (command.equals("CMD_0")) {
           display.setText(display.getText() + "0");
        } else
        if (command.equals("CMD_DOT")) {
            display.setText(display.getText() + ".");
        } else
        if (command.equals("CMD_PLUSMINUS")) {
                display.setText("-" + display.getText());
        } else
        if (command.equals("CMD_DEL")) {
         if 
            (x == 0) {
                int len = display.getText().length();
            display.setText(display.getText().substring(0, len-1));
        }
        } else
       if (command.equals("CMD_ADD")) {
            if (display.getText().equals("")) {
                display.setText("");
                temp = 0;
                ch = '+';
            } else {
                temp = Double.parseDouble(display.getText());
                display.setText("");
                ch = '+';
                y = 0;
                x = 0;
            }
            display.requestFocus();
            } else
        if (command.equals("CMD_MIN")) {
            if (display.getText().equals("")) {
                display.setText("");
                temp = 0;
                ch = '-';
            } else {
                x = 0;
                y = 0;
                temp = Double.parseDouble(display.getText());
                display.setText("");
                ch = '-';
            }
            display.requestFocus();
            } else
        if (command.equals("CMD_MUL")) {
            if (display.getText().equals("")) {
                display.setText("");
                temp = 1;
                ch = '*';
            } else {
                x = 0;
                y = 0;
                temp = Double.parseDouble(display.getText());
                ch = '*';
                display.setText("");
            }
            display.requestFocus();
            } else
        if (command.equals("CMD_DIV")) {
            if (display.getText().equals("")) {
                display.setText("");
                temp = 1;
                ch = '/';
            } else {
                x = 0;
                y = 0;
                temp = Double.parseDouble(display.getText());
                ch = '/';
                display.setText("");
            }
            display.requestFocus();
            } else
        if (command.equals("CMD_LEFT")) {
            display.setText("");
            display.setText(display.getText() + "( function is not valid");
            } else
        if (command.equals("CMD_RIGHT")) {
            display.setText("");
            display.setText(display.getText() + ") function is not valid");
        } else
        if (command.equals("CMD_INT")) {
            if (display.getText().equals("")) {
                display.setText("");
            } else {
                a = fact(Double.parseDouble(display.getText()));
                display.setText("");
                display.setText(display.getText() + a);
            }
        display.requestFocus();
        } else
        if (command.equals("CMD_C")) {
            display.setText("");
            x = 0;
            y = 0;
            z = 0;
        } else
        if (command.equals("CMD_ANSW")) {
            if (display.getText().equals("")) {
                display.setText("");
            } else {
                temp1 = Double.parseDouble(display.getText());
                switch (ch) {
                case '+':
                    result = temp + temp1;
                    break;
                case '-':
                    result = temp - temp1;
                    break;
                case '/':
                    result = temp / temp1;
                    break;
                case '*':
                    result = temp * temp1;
                    break;
                }
                display.setText("");
                display.setText(display.getText() + result);
                z = 1;
            }
        
        } else
        if (command.equals("CMD_OFF")) {
            System.exit(0);
            } else
        display.setText(displayText);
    }
    
    //factorial method
    double fact(double x) {
        int er = 0;
        if (x < 0) {
            er = 20;
            return 0;
        }
        double i, s = 1;
        for (i = 2; i <= x; i += 1.0)
            s *= i;
        return s;
    }
    
    //main method to run the program
    public static void main (String[] args) {
        SimpleCalc calc = new  SimpleCalc();
        calc.setVisible(true);
    }
}
