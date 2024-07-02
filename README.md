import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class Main extends JFrame {
    private JTextField pantalla;
    private double operando1, operando2;
    private char operador;

    public Main() {
        setTitle("Calculadora");
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setSize(300, 400);
        setLocationRelativeTo(null);

        JPanel panel = new JPanel();
        panel.setLayout(new BorderLayout());

        pantalla = new JTextField();
        pantalla.setEditable(false);
        pantalla.setFont(new Font("Arial", Font.PLAIN, 24)); // Aumenta el tamaño de la fuente
        pantalla.setHorizontalAlignment(JTextField.RIGHT); // Alinea el texto a la derecha
        panel.add(pantalla, BorderLayout.NORTH);

        JPanel teclado = new JPanel();
        teclado.setLayout(new GridLayout(4, 4, 10, 10)); // Añade más espacio entre los botones

        JButton[] botones = new JButton[16];
        String[] labels = {"7", "8", "9", "/",
                "4", "5", "6", "*",
                "1", "2", "3", "-",
                "0", "C", "=", "+"};

        for (int i = 0; i < 16; i++) {
            botones[i] = new JButton(labels[i]);
            teclado.add(botones[i]);
            botones[i].setMargin(new Insets(20, 20, 20, 20)); // Aumenta el margen interno de los botones
            botones[i].setFont(new Font("Arial", Font.PLAIN, 18)); // Aumenta el tamaño de la fuente de los botones
            botones[i].addActionListener(new ActionListener() {
                public void actionPerformed(ActionEvent e) {
                    JButton boton = (JButton) e.getSource();
                    String textoBoton = boton.getText();

                    if (textoBoton.equals("C")) {
                        pantalla.setText("");
                    } else if (esOperador(textoBoton)) {
                        operador = textoBoton.charAt(0);
                        operando1 = Double.parseDouble(pantalla.getText());
                        pantalla.setText(pantalla.getText() + " " + textoBoton + " "); // Agrega espacio alrededor del operador
                    } else if (textoBoton.equals("=")) {
                        operando2 = Double.parseDouble(pantalla.getText().substring(pantalla.getText().indexOf(operador) + 2));
                        double resultado = calcular();
                        pantalla.setText(pantalla.getText() + " = " + resultado); // Muestra el resultado
                    } else {
                        pantalla.setText(pantalla.getText() + textoBoton);
                    }
                }
            });
        }

        panel.add(teclado, BorderLayout.CENTER);
        add(panel);
    }

    private boolean esOperador(String texto) {
        return texto.equals("+") || texto.equals("-") || texto.equals("*") || texto.equals("/");
    }

    private double calcular() {
        double resultado = 0;
        switch (operador) {
            case '+':
                resultado = operando1 + operando2;
                break;
            case '-':
                resultado = operando1 - operando2;
                break;
            case '*':
                resultado = operando1 * operando2;
                break;
            case '/':
                if (operando2 != 0)
                    resultado = operando1 / operando2;
                else
                    JOptionPane.showMessageDialog(this, "Error: división por cero");
                break;
        }
        return resultado;
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(new Runnable() {
            public void run() {
                Main calc = new Main();
                calc.setVisible(true);
            }
        });
    }
}
