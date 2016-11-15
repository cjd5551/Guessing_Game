// Author   : Christian Dunn
// Date     : Feb. 17th 2016
// Purpose  : To manually create a guessing game gui that allows the user to guess
//            a randomly generated number based on hints from the program
// Compiler : Eclipse

import java.awt.*;
import javax.swing.*;
import javax.swing.text.BadLocationException;
import javax.swing.text.JTextComponent;
import java.util.Random;
import java.util.logging.Logger;
import java.awt.event.*;

// Class   : GuessingGame
// Purpose : Create a JFrame used in running the main GUI for GuessingGame
public class GuessingGame extends JFrame
{

    private static final long serialVersionUID = 1L;
    private JButton guessBtn;
    private JButton restartBtn;
    private JPanel panel;
    private JTextField numberFld;
    private JLabel message;
    private int guess = 0;
    private int random;

    // Method     : main
    // Purpose    : To create GuessingGame ubject and run main GUI program
    // Parameters : -> GuessingGame myFrame
    public static void main(String[] args) 
	    {
	
	       GuessingGame myFrame = new GuessingGame();
	
	       myFrame.setSize(600,400);//dimensions of frame
	       myFrame.setVisible(true); //can be seen
	
	       myFrame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);//closes program
	
	    }//ends main

   // Method     : GuessingGame
   // Purpose    : To act as constructor for GuessingGame JFrame and GUI components
   // Parameters : Color              ColBlue    -> Creates object of color blue
   //              Color              ColRed     -> Creates object of color red
   //              Color              ColGreen   -> Creates object of color green
   //              int                hot        -> Range considered hot within actual number
   //              int                cold       -> Range considered cold within actual number
   //              int                min        -> Minimum possible number
   //              int                max        -> Maximum possible number
   //              GridBagConstraints gbc        -> Creates constraints for layout manager
   //              JTextArea          textArea   -> TextArea to hold memory of guesses
   //              JScrollPane        scrollPane -> ScrollPane to keep size of textArea constant
   //              Random             generator  -> Random object to generate random number within min and max
   public GuessingGame()
	   {
		Color ColBlue      = new Color(150, 150, 255);
		Color ColRed       = new Color(255, 150, 150);
		Color ColGreen     = new Color(150, 255, 150);
		int hot = 25, cold = 50, min = 1, max = 1000 ;
	   
	       setLayout(new GridBagLayout());
	       setTitle("Guessing Game"); // title on top bar
	
               setLayout(new GridBagLayout());
               GridBagConstraints gbc = new GridBagConstraints();
               gbc.gridx = 0;
               gbc.gridy = 0;
               gbc.insets = new Insets(4, 4, 4, 4);
	       
	       panel = new JPanel();
	       
	       message = new JLabel("Guess a number between 1 and 1000:");
	       gbc.gridx = 0;
	       gbc.gridy = 0;
	       add(message, gbc);

	       numberFld = new JTextField(10); // adds text field
	       gbc.gridx = 0; 
	       gbc.gridy = 1;
	       add(numberFld, gbc);
	       
	       guessBtn = new JButton("ENTER"); // button to enter guess number
	       gbc.gridx = 1;
	       gbc.gridy = 1;
	       add(guessBtn, gbc);
	
	       restartBtn = new JButton("NEW GAME"); // button to start from the beginning
	       gbc.gridy = 1;
	       gbc.gridx = 2;
	       add(restartBtn, gbc);
	       
	       JTextArea textArea = new JTextArea(8, 10);
	       JScrollPane scrollPane = new JScrollPane(textArea);
	       gbc.gridy = 3;
	       gbc.gridx = 0;
	       add(scrollPane, gbc);
	       textArea.setEditable(false);
	       
	       JLabel areaTitle = new JLabel("Guesses:");
	       gbc.gridx = 0;
	       gbc.gridy = 2;
	       add(areaTitle, gbc);
	       
	       guessBtn.addActionListener(new ActionListener(){
			    @Override
			    public void actionPerformed(ActionEvent event)
				    {
			    		guess = Integer.parseInt(numberFld.getText());
			    		int diff = guess - random;
					int difference = Math.abs(diff);
			    		
			    		textArea.append(Integer.toString(guess) + "\n");
			    		
					    if (guess == random)
						    {
						       message.setText("Correct");
						       numberFld.setEditable(false);
						       numberFld.setBackground(ColGreen);
						    }//end if
				    
					    if (difference <= hot)
					    	numberFld.setBackground(ColRed);
					    
				    	if ((difference > hot) && (difference <= cold))
				    		numberFld.setBackground(ColBlue);
					    
					    if (guess > random)
					    	message.setText("You are too high!");
				
					    if (guess < random)
					    	message.setText("You are too low");
					    
					    if ((guess > max) || (guess < min))
					    	message.setText("Invalid Input! Range 1 - 1000");
				    }
	       });
	       
	       numberFld.addActionListener(new ActionListener(){
			    @Override
			    public void actionPerformed(ActionEvent event)
				    {
					    guess = Integer.parseInt(numberFld.getText());
					    int diff = guess - random;
					    int difference = Math.abs(diff);
					    
					    textArea.append(Integer.toString(guess) + "\n");

					    if (guess == random)
						    {
						       message.setText("Correct");
						       numberFld.setEditable(false);
						       numberFld.setBackground(ColGreen);
						    }//end if
					    
					    if (difference <= hot)
						    numberFld.setBackground(ColRed);
					    
					    if ((difference > hot) && (difference <= cold))
						    numberFld.setBackground(ColBlue);
						    
					    if (guess > random)
					    	message.setText("You are too high!");
				
					    if (guess < random)
					    	message.setText("You are too low");  
					    
					    if ((guess > max) || (guess < min))
					    	message.setText("Invalid Input! Range 1 - 1000");
				    }//ends event
	       });

	       Random generator = new Random();
	       random = generator.nextInt(1001)+1;
	
	       restartBtn.addActionListener(
	
	       new ActionListener() // anonymous inner class
	       		{
			
			      public void actionPerformed(ActionEvent e)
				      { 
                                          theGame(); 
                                          textArea.setText(" ");
                                      } // end method
			
			    } //end action listener
	
	    	); // end action listener
	
	   }///ends guess the numbers constructor
   
   // Method  : theGame
   // Purpose : used to reset the game
   public void theGame()
	 {
	   	numberFld.setText("");
		numberFld.setEditable(true);
		numberFld.setBackground(Color.white);
		
		Random generator = new Random();
		random = generator.nextInt(1001)+1;
		message.setText("Guess a number between 1 and 1000:");
		
		guess = 0;
	 }//end theGame

} // End GuessingGame