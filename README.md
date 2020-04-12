# my first project ever tictactoe
import java.util.Arrays;
import java.util.Scanner; 

public class A3 {
	private static Scanner scanner;
	
	static int dimensions;

	//making a global variable in order to reference throughout the entire assignment as opposed
	// to referencing it each time 
	
	public static void main(String[] args) {
		dimensions = 3;
		char[][] board=CreateBoard(dimensions);
		 // char[][] board= {{' ',' ', 'x'},{' ',' ',' '},{' ',' ',' '}}; 
		//  getUsersMove(board); 
		// writeOnBoard(board,0,0,'x');
		 // displayBoard(board);
		 //checkForObviousMove(board); 
		 //displayBoard(board);
		 //getAImove(board);
		 play(); 
		
		/*char[][] board2 = CreateBoard(3);
		displayBoard(board2);
		writeOnBoard(board,0,0,'x');
		writeOnBoard(board,1,1,'x'); */
		
		
	}
	public static void play() { 
		int user = 'x'; 
		int computer = 'o'; 
		
		Scanner sc = new Scanner(System.in); 
		
		System.out.println("Hello, what is your name?");
		
		 String name = sc.next();
		
		System.out.println("What would you like the dimensions of the board to be?");
		
		 if(sc.hasNextInt()) 
		 dimensions = sc.nextInt(); 
		 
		int coinToss = 0 + (int)(Math.random() * 1);
		char[][] board=CreateBoard(dimensions);
		int check=0;
		while(check==0) { 
			getUsersMove(board);
			getAImove(board);
		if(getWinner(board) == 'x') { 
		System.out.println(name + "has won the game");
		check=1;
		} if(getWinner(board) == 'o') {
		 System.out.println("the AI has won");
		 check=1;
		}	
	}
} 
	

		 
 
	 
	
	
	public static char[][] CreateBoard(int n) {
		char[][] board = new char[n][n];
		for (int i = 0; i < board.length; i++) {
			for (int j = 0; j < board.length; j++) {
				board[i][j] = ' ';
			}
		}
		//System.out.println(Arrays.deepToString(board));
		return board;
	}

	public static void displayBoard(char[][] board) {

		int D=0;
		int Q=0;
		for(int i=1; i<=board.length*2+1;i++){
			if(i%2!=0){ //if the row is odd 
				for(int k=1;k<=board.length*2+1;k++) { //prints plus and minus
						if(k%2!=0){
							System.out.print('+');
						}
						else{
							System.out.print('-');
						}
					
				}
			}
			else{ //if the row is even 
				for(int j=1;j<=board.length*2+1;j++) {
					if(j%2!=0){
						System.out.print('|');
					}
					else{
						if(Q>board.length-1) {
							Q=0;
							D++;
						}
						System.out.print(board[D][Q]);
						Q++;
					//where you would print the board values
					}
				}	
			}
			System.out.println(); //print new line
		}
	}
	
	public static void writeOnBoard(char[][] board, int rows, int columns, char symbol) { 
	
		if((board.length - 1 < rows) || (board.length - 1 < columns)) throw new IllegalArgumentException("Error: that is not on the board");	
		
		if(board[rows][columns] !=' ') throw new IllegalArgumentException("Error: Your Opponent has Already Written There");
			
		board[rows][columns] = symbol; 
		
		displayBoard(board);
	} 

	public static void getUsersMove(char[][] board) {
		 scanner = new Scanner(System.in);
		    System.out.println("which row would you like to play in?");
		    
		    int row = scanner.nextInt(); 
		    
		    System.out.println("you have chosen row number" + row);
		    
		    System.out.println("which column would you like to play in?");
		    
		    int column = scanner.nextInt();
		    
		    System.out.println("You have chosen column number "+ column);
		    
		    writeOnBoard(board, row, column, 'x'); }
		  
	
	public static boolean checkForObviousMove(char[] [] board) { //
		for(int i = 0; i < board.length; i++) { 
			char[] temp = turnRowIntoSingleArray(board, i);
			if(checkTwoOfAKindCompSymbol(temp)) {
				fillEmptyRowSpace(temp, board, i); 
			return true; }
			temp = turnColumnIntoSingleArray(board, i);
			if(checkTwoOfAKindCompSymbol(temp)) {
				fillEmptyColumnSpace(temp, board, i); 
				return true; }
			if (checkMaindiagonal('o',board)) {
					fillEmptyDiagonalSpace(temp, board, i); 
					return true; 
			}
			 if (checkAntiDiagonal('o', board)) {
					fillEmptyDiagonalSpace(temp, board, i); 
					return true; }
			}
	
		for(int j = 0; j < board.length; j++) { 
			char[] tempTwo = turnRowIntoSingleArray(board, j);
			
			if(checkTwoOfAKindUserSymbol(tempTwo)) {
				fillEmptyRowSpace(tempTwo, board, j); 
				return true; 
			}
			
			tempTwo = turnColumnIntoSingleArray(board, j);
			if(checkTwoOfAKindUserSymbol(tempTwo)) {
				fillEmptyColumnSpace(tempTwo, board, j); 
				return true; 
			}
			if (checkMaindiagonal('o',board)) {
				tempTwo = turnDiagonalIntoSingleArray(board); {
					fillEmptyDiagonalSpace(tempTwo, board, j); 
					return true;
				}
			}
			else if (j == 1) {
				tempTwo = turnDiagonalTwoIntoSingleArray(board);
				if(checkTwoOfAKindUserSymbol(tempTwo)) {
					fillEmptyDiagonalSpace(tempTwo, board, j); 
					return true; 
				}
			}
		}
		return false; 
	}
	
	public static void getAImove(char[][] board) { 
		
		if(!checkForObviousMove(board)){
		
			int coordinate1 = (int) (Math.random() * dimensions);
			int coordinate2 = (int) (Math.random() * dimensions); 
			
			while(!checkifEmpty(board,coordinate1,coordinate2)) {
				 coordinate1 = (int) (Math.random() * dimensions);
				 coordinate2 = (int) (Math.random() * dimensions);
					
		}
			writeOnBoard(board, coordinate1, coordinate2, 'o'); 	
	}}

	public static char getWinner(char [][] board) {
		int counterX = 0;
		int counterO = 0; 
		for(int i = 0; i< dimensions; i++)
		{ 
			 char [] rowX = turnRowIntoSingleArray(board, i);
			 char [] columnY = turnColumnIntoSingleArray(board, i);
			for(int j = 0; j < dimensions; j++) { 
				if(rowX[j]=='x')
				if(columnY[j] == 'x')
				{
					counterX ++; 
				}
				else if(rowX[j]== 'o')
				if(columnY[j] == 'o'){
					counterO++; 
				}
				if(counterX == dimensions) {
					return 'x'; 
				}
					if(counterO == dimensions) {
					return 'o';
				}
			}
			}
			int counter1 = 0;
			int counter2 = 0;		
		 char [] diagonal1 = turnDiagonalIntoSingleArray(board);
		 char [] diagonal2 = turnDiagonalTwoIntoSingleArray(board);
		 for(int k = 0; k < dimensions; k++) {
			 if(diagonal1[k] == 'x')
				 if(diagonal2[k] == 'x')
				 {
					 counter1 ++; 
				 }
				 else if(diagonal1[k] == 'o')
					 if(diagonal2[k] == 'o') { 
		 		}
			 		counter2 ++; 
		 }
		 	
		return ' '; 
		}
		
				
	public static char[] turnRowIntoSingleArray(char [][] board, int rowPos){   

		char [] singleRow = new char[board.length];
		for(int i = 0; i < board.length; i++) { 
		singleRow[i] = board[rowPos][i];
		}
		return singleRow;
	}
	public static char[] turnColumnIntoSingleArray(char [][] board, int ColumnPos){   

		char [] singleColumn = new char[board.length];
		for(int j = 0; j < board.length; j++) { 
		singleColumn[j] = board[j][ColumnPos];
		}
		return singleColumn;
	} 
	
	public static char[] turnDiagonalIntoSingleArray(char [][] board){   
		char[] DiagonalArray = new char[board.length];
		for(int i = 0;i<board.length;i++ ) {
			DiagonalArray[i] = board[i][i]; 
		} 
		return DiagonalArray; 		
		}
	
	public static char[] turnDiagonalTwoIntoSingleArray(char [][] board){   
		
		int counter = board.length-1; 
		
		char[] DiagonalArray = new char[board.length]; 
		for(int i = board.length - 1;i>=0; i-- ) {
			DiagonalArray[i] = board[counter][i]; 
			counter --;
		} 
		return DiagonalArray; 		
		}
		

	
//the following helpers perform 2 tasks. One of them finds two of a kind (x's or o's to determine if there is an obvious move to make) and the fillEmptyColumn/Row/Diagonal actively 
//fill the missing gap with the obvious move either blocking or winning the game
	public static boolean checkTwoOfAKindUserSymbol(char [] singleRow) { 
		int counter = 0; 
		
		for(int i = 0; i < singleRow.length; i++) { 
		if(singleRow[i] == 'x')
			counter++;
		if(singleRow[i] == 'o') 
			return false;
		}
		if(counter== dimensions - 1) 
			return true;
		else
			return false;
}
	public static boolean checkTwoOfAKindCompSymbol(char [] singleRow) { 
		int counter = 0; 
		boolean bool = false;
		
		for(int i = 0; i < singleRow.length; i++) { 
		if(singleRow[i] == 'o') {
			counter++;}
		if(singleRow[i] == 'x') {
			bool = false;
		}
		if(counter==dimensions - 1 ) { 
			bool = true;
			}
		else {
			bool = false;
			}
	}
	return bool;	
	}
	public static void fillEmptyRowSpace(char [] singleRow, char [][] board, int columnNumberToFill) { 
		
		for(int j = 0; j < singleRow.length; j++) { 
			if(singleRow[j] == ' ') {
				writeOnBoard(board, columnNumberToFill, j,  'o');
				} 
		}
				
		}
	public static void fillEmptyColumnSpace(char [] singleRow, char [][] board, int rowNumberToFill) { 
		
				for(int j = 0; j < singleRow.length; j++) { 
					if(singleRow[j] == ' ') {
						writeOnBoard(board,j, rowNumberToFill, 'o'); 
					}
				}
			
	}
	public static void fillEmptyDiagonalSpace(char [] singleRow, char [][] board, int diagonalNumber) { 
			
			for(int i=0; i < singleRow.length; i++) {		
				if(singleRow[i]== ' ') {
					writeOnBoard(board,i, diagonalNumber, 'o');
				}
				}}
	public static boolean checkAntiDiagonalWinner(char c, char [][] board) {
		int counter = 0; 
		int j =0; 
		for(int i = board.length-1 ; i >= 0; i --) {
			if(board[i][j]==c) {
				counter++;
			}
			j++;
		}
		if(counter == board.length) {
			return true;
		}else {
			return false;
		}
			}
	public static boolean checkAntiDiagonal(char c, char[][] board) {
		int counter = 0;
		int j = 0 ;
		for(int i = board.length - 1; i>= 0; i--) {
			if(board[i][j]==c) {
				counter++;
			}
			j++;
			}
		if(counter == board.length-1) {
			return true;
		}else {
			return false; 
		}
		}
	public static boolean checkMainDiagonalWinner(char c, char [][] board) {
		int counter = 0;
		int j=0;
		for(int i =0; i < board.length; i++) {
			if(board[i][j] == c) {
				counter ++;			
			}
			j++;
		}
		if(counter ==board.length-1) {
			return true;
		}else {
			return false; 
		}
	}
	public static boolean checkMaindiagonal(char c, char[][] board) {
		int counter = 0;
		int j = 0;
		for(int i = 0; i < board.length; i++) {
			if(board[i][j] == c) {
				counter++;
			}
			j++;
			}
		if(counter==board.length-1) {
			return true;
		}else {
			return false;
		}
	
	}

	public static boolean checkifEmpty(char[][]board, int coordinate1, int coordinate2) {
		boolean bool = true;
		if(board[coordinate1][coordinate2] !=' ') {
			bool = false; 
		}
		return bool;
	}}
