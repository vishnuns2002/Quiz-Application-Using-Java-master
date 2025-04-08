package quiz.application;

import javax.swing.*;
import java.awt.*;
import java.awt.event.*;

public class Quiz extends JFrame implements ActionListener {
    public static String[][] questions = new String[10][5];
    public static String[][] answers = new String[10][2];
    public static String[][] useranswers = new String[10][1];

    JLabel qno, question, timerLabel;
    JRadioButton opt1, opt2, opt3, opt4;
    ButtonGroup groupoptions;
    JButton next, submit, lifeline;

    public static int count = 0;
    public static int score = 0;
    public static int timeLeft = 15;
    Timer timer;

    String name;

    Quiz(String name) {
        this.name = name;
        count = 0;
        score = 0;
        timeLeft = 15;

        setBounds(50, 0, 1440, 850);
        getContentPane().setBackground(Color.WHITE);
        setLayout(null);

        ImageIcon i1 = new ImageIcon(ClassLoader.getSystemResource("icons/quiz.jpg"));
        JLabel image = new JLabel(i1);
        image.setBounds(0, 0, 1440, 392);
        add(image);

        qno = new JLabel();
        qno.setBounds(100, 450, 50, 30);
        qno.setFont(new Font("Tahoma", Font.PLAIN, 24));
        add(qno);

        question = new JLabel();
        question.setBounds(150, 450, 900, 30);
        question.setFont(new Font("Tahoma", Font.PLAIN, 24));
        add(question);

        timerLabel = new JLabel("Time left - 15 seconds");
        timerLabel.setBounds(1100, 500, 300, 30);
        timerLabel.setFont(new Font("Tahoma", Font.BOLD, 25));
        timerLabel.setForeground(Color.RED);
        add(timerLabel);

        opt1 = new JRadioButton();
        opt2 = new JRadioButton();
        opt3 = new JRadioButton();
        opt4 = new JRadioButton();

        JRadioButton[] options = {opt1, opt2, opt3, opt4};
        int y = 520;
        for (JRadioButton opt : options) {
            opt.setBounds(170, y, 700, 30);
            opt.setBackground(Color.WHITE);
            opt.setFont(new Font("Dialog", Font.PLAIN, 20));
            add(opt);
            y += 40;
        }

        groupoptions = new ButtonGroup();
        groupoptions.add(opt1);
        groupoptions.add(opt2);
        groupoptions.add(opt3);
        groupoptions.add(opt4);

        next = new JButton("Next");
        next.setBounds(1100, 550, 200, 40);
        next.setFont(new Font("Tahoma", Font.PLAIN, 22));
        next.setBackground(new Color(30, 144, 255));
        next.setForeground(Color.WHITE);
        next.addActionListener(this);
        add(next);

        lifeline = new JButton("50-50 Lifeline");
        lifeline.setBounds(1100, 630, 200, 40);
        lifeline.setFont(new Font("Tahoma", Font.PLAIN, 22));
        lifeline.setBackground(new Color(30, 144, 255));
        lifeline.setForeground(Color.WHITE);
        lifeline.addActionListener(this);
        add(lifeline);

        submit = new JButton("Submit");
        submit.setBounds(1100, 710, 200, 40);
        submit.setFont(new Font("Tahoma", Font.PLAIN, 22));
        submit.setBackground(new Color(30, 144, 255));
        submit.setForeground(Color.WHITE);
        submit.addActionListener(this);
        submit.setEnabled(false);
        add(submit);

        setupQuestions();
        start(count);
        startTimer();

        setVisible(true);
    }

    private void setupQuestions() {
        // (Same question setup as you had before — unchanged for brevity)
        questions[0][0] = "Which is used to find and fix bugs in the Java programs.?";
        questions[0][1] = "JVM"; questions[0][2] = "JDB"; questions[0][3] = "JDK"; questions[0][4] = "JRE";
        questions[1][0] = "What is the return type of the hashCode() method in the Object class?";
        questions[1][1] = "int"; questions[1][2] = "Object"; questions[1][3] = "long"; questions[1][4] = "void";
        questions[2][0] = "Which package contains the Random class?";
        questions[2][1] = "java.util package"; questions[2][2] = "java.lang package"; questions[2][3] = "java.awt package"; questions[2][4] = "java.io package";
        questions[3][0] = "An interface with no fields or methods is known as?";
        questions[3][1] = "Runnable Interface"; questions[3][2] = "Abstract Interface"; questions[3][3] = "Marker Interface"; questions[3][4] = "CharSequence Interface";
        questions[4][0] = "In which memory a String is stored, when we create a string using new operator?";
        questions[4][1] = "Stack"; questions[4][2] = "String memory"; questions[4][3] = "Random storage space"; questions[4][4] = "Heap memory";
        questions[5][0] = "Which of the following is a marker interface?";
        questions[5][1] = "Runnable interface"; questions[5][2] = "Remote interface"; questions[5][3] = "Readable interface"; questions[5][4] = "Result interface";
        questions[6][0] = "Which keyword is used for accessing the features of a package?";
        questions[6][1] = "import"; questions[6][2] = "package"; questions[6][3] = "extends"; questions[6][4] = "export";
        questions[7][0] = "In java, jar stands for?";
        questions[7][1] = "Java Archive Runner"; questions[7][2] = "Java Archive"; questions[7][3] = "Java Application Resource"; questions[7][4] = "Java Application Runner";
        questions[8][0] = "Which of the following is a mutable class in java?";
        questions[8][1] = "java.lang.StringBuilder"; questions[8][2] = "java.lang.Short"; questions[8][3] = "java.lang.Byte"; questions[8][4] = "java.lang.String";
        questions[9][0] = "Which of the following option leads to the portability and security of Java?";
        questions[9][1] = "Bytecode is executed by JVM"; questions[9][2] = "The applet makes the Java code secure and portable"; questions[9][3] = "Use of exception handling"; questions[9][4] = "Dynamic binding between objects";

        answers[0][1] = "JDB"; answers[1][1] = "int"; answers[2][1] = "java.util package";
        answers[3][1] = "Marker Interface"; answers[4][1] = "Heap memory"; answers[5][1] = "Remote interface";
        answers[6][1] = "import"; answers[7][1] = "Java Archive"; answers[8][1] = "java.lang.StringBuilder";
        answers[9][1] = "Bytecode is executed by JVM";
    }

    private void start(int count) {
        qno.setText((count + 1) + ". ");
        question.setText(questions[count][0]);

        opt1.setText(questions[count][1]); opt1.setActionCommand(questions[count][1]);
        opt2.setText(questions[count][2]); opt2.setActionCommand(questions[count][2]);
        opt3.setText(questions[count][3]); opt3.setActionCommand(questions[count][3]);
        opt4.setText(questions[count][4]); opt4.setActionCommand(questions[count][4]);

        groupoptions.clearSelection();
        opt1.setEnabled(true); opt2.setEnabled(true); opt3.setEnabled(true); opt4.setEnabled(true);

        timeLeft = 15;
        startTimer();
    }

    private void saveAnswer() {
        if (groupoptions.getSelection() == null) {
            useranswers[count][0] = "";
        } else {
            useranswers[count][0] = groupoptions.getSelection().getActionCommand();
        }
    }

    private void applyLifeline() {
        String correct = answers[count][1];
        int hidden = 0;
        for (JRadioButton opt : new JRadioButton[]{opt1, opt2, opt3, opt4}) {
            if (!opt.getText().equals(correct) && hidden < 2) {
                opt.setEnabled(false);
                hidden++;
            }
        }
    }

    private void calculateScore() {
        for (int i = 0; i < useranswers.length; i++) {
            if (useranswers[i][0] != null && useranswers[i][0].equals(answers[i][1])) {
                score += 10;
            }
        }
    }

    private void startTimer() {
        if (timer != null) timer.stop();

        timer = new Timer(1000, e -> {
            timerLabel.setText("Time left - " + timeLeft + " seconds");
            timeLeft--;

            if (timeLeft < 0) {
                saveAnswer();
                if (count == 9) {
                    timer.stop();
                    calculateScore();
                    setVisible(false);
                    new Score(name, score);
                } else {
                    count++;
                    start(count);
                }
            }
        });

        timer.start();
    }

    public void actionPerformed(ActionEvent ae) {
        if (ae.getSource() == next) {
            timer.stop();
            saveAnswer();
            if (count == 8) {
                next.setEnabled(false);
                submit.setEnabled(true);
            }
            count++;
            start(count);
        } else if (ae.getSource() == lifeline) {
            applyLifeline();
            lifeline.setEnabled(false);
        } else if (ae.getSource() == submit) {
            timer.stop();
            saveAnswer();
            calculateScore();
            setVisible(false);
            new Score(name, score);
        }
    }

    public static void main(String[] args) {
        new Quiz("User");
    }
}
