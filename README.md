# Stop-clock-watch
import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.text.SimpleDateFormat;
import java.util.Date;

public class ClockStopwatchApp extends JFrame implements ActionListener {
    JLabel clockLabel, stopwatchLabel;
    JButton startBtn, stopBtn, resetBtn;

    Timer clockTimer, stopwatchTimer;
    int elapsedTime = 0;
    boolean isRunning = false;

    public ClockStopwatchApp() {
        setTitle("Clock & Stopwatch");
        setSize(300, 200);
        setLayout(new GridLayout(4, 1));

        // Clock
        clockLabel = new JLabel("", SwingConstants.CENTER);
        clockLabel.setFont(new Font("Verdana", Font.PLAIN, 18));
        add(clockLabel);

        // Stopwatch
        stopwatchLabel = new JLabel("00:00:00", SwingConstants.CENTER);
        stopwatchLabel.setFont(new Font("Verdana", Font.BOLD, 24));
        add(stopwatchLabel);

        // Buttons
        JPanel buttonPanel = new JPanel();
        startBtn = new JButton("Start");
        stopBtn = new JButton("Stop");
        resetBtn = new JButton("Reset");
        buttonPanel.add(startBtn);
        buttonPanel.add(stopBtn);
        buttonPanel.add(resetBtn);
        add(buttonPanel);

        startBtn.addActionListener(this);
        stopBtn.addActionListener(this);
        resetBtn.addActionListener(this);

        // Clock Timer (updates every second)
        clockTimer = new Timer(1000, e -> updateClock());
        clockTimer.start();

        // Stopwatch Timer (updates every second)
        stopwatchTimer = new Timer(1000, e -> {
            elapsedTime += 1000;
            stopwatchLabel.setText(formatTime(elapsedTime));
        });

        setDefaultCloseOperation(EXIT_ON_CLOSE);
        setVisible(true);
    }

    void updateClock() {
        String time = new SimpleDateFormat("HH:mm:ss").format(new Date());
        clockLabel.setText("Time: " + time);
    }

    String formatTime(int millis) {
        int seconds = (millis / 1000) % 60;
        int minutes = (millis / 60000) % 60;
        int hours = (millis / 3600000);
        return String.format("%02d:%02d:%02d", hours, minutes, seconds);
    }

    @Override
    public void actionPerformed(ActionEvent e) {
        if (e.getSource() == startBtn) {
            if (!isRunning) {
                stopwatchTimer.start();
                isRunning = true;
            }
        } else if (e.getSource() == stopBtn) {
            stopwatchTimer.stop();
            isRunning = false;
        } else if (e.getSource() == resetBtn) {
            stopwatchTimer.stop();
            elapsedTime = 0;
            stopwatchLabel.setText("00:00:00");
            isRunning = false;
        }
    }

    public static void main(String[] args) {
        new ClockStopwatchApp();
    }
}
