package jason_progapMP;

import java.util.*;
import java.io.*;
import java.util.Date;

public class progapmp {


  public static void main(String[] args) {
		print obj = new print();
		obj.random();
		obj.ReadandPrint();
		}
}
	
class print{
	
	int iRan[]=new int[6]; 
	
	public void random(){
		int dup=0;
		Random gen = new Random();
		do{
			for(int iCtr=0;iCtr<6;iCtr++){
				int r = gen.nextInt(48)+1;
				iRan[iCtr]= r;
			}dup = check(iRan);
		}while(dup == 1);
	}
	
	public static int check(int [] arr){
		int dup=0;
			 for (int i = 0; i < arr.length; i++ ) {
		            for (int iC = i + 1; iC < arr.length; iC++){
		            	if( arr[iC]==arr[i] && arr[iC]!=0 && arr[i] != 0 ){
		            		dup = 1;
		            	}
		            }
		     }
        return dup;
    }
	
	public void ReadandPrint(){
		String input = Reader.readInput("File directory: "),strLine;
		int iCtr=0;
		Line lineobj = new Line();
		try{
			FileInputStream inputStream = new FileInputStream(input);
			DataInputStream dataInput = new DataInputStream(inputStream);
			BufferedReader br = new BufferedReader(new InputStreamReader(dataInput));
			try{
				for(int ran:iRan){
					System.out.print(ran + " ");
				}
				System.out.print("\n");
				while((strLine = br.readLine()) != null){
					iCtr++;
					lineobj.text(strLine);
					lineobj.number(strLine);
				}
				int iplayernum = lineobj.iplayernum, earnnings= lineobj.earnnings;
				String strplayer = lineobj.strplayers,strwinners=lineobj.winner;
				System.out.println("Number of player: " + iplayernum);
				System.out.println(strplayer);
				System.out.println("Total Earnings: " + earnnings * 10);
				System.out.println(strwinners);
			}catch(IOException ioe){
				System.out.println("error" + ioe);
			}
		}catch(FileNotFoundException fnfe){
			System.out.println("FileNotFoundException: " + fnfe.getMessage());
		}
		
		
	}
		
}

class Line{
	int iplayernum,playernum[]=new int[20], earnnings=0;
	String strplayers="",strWin="",winner="";
	double iword;
	
	public void text(String str){
		String [] names = str.split(" ");
		int l = str.length(),ic;
		for(int i=0;i<l;i++){
			char c = str.charAt(i); 
			ic=(int)c;
			if(ic>=65 && ic<=122 || ic==32 ){
				strplayers += c;
				if(i==l-1){
				iplayernum++;
				strplayers += "\n";
				}
			}else if(ic>=48 && ic<=57){
				break;
			}
		}
	}
	
	public void number(String str){
		print obj = new print();
		String [] num = str.split(" ");
		boolean Number; 
		double enterCount;
		int integer, iArray[]=new int[20],reject=0,iRan[]=obj.iRan,comboCount;
		iword = wordcount(str);
		for(int iC=0;iC<iword;iC++){
			Number=isNumeric(num[iC]);
			if(Number == true){
				if(iword == 6){
					integer = Integer.parseInt(num[iC]);
					if(integer > 0 && integer < 50 ){
						iArray[iC]= integer;
					}else{
						reject=1;
						break;
					}
				}else{
					reject=1;
					break;
				}
			}else if(Number == false){
				reject=1;
				break;
			}
		}
		if(reject != 1){
			reject = duplicatecheck(iArray);
			}
		if(reject == 0){
			comboCount = combocheck(iArray,iRan);
			
			if(comboCount == 3){
				strWin = "20";
			}else if(comboCount == 4){
				strWin = "500";
			}else if(comboCount == 5){
				strWin = "20,000";
			}else if(comboCount == 6){
				strWin = "100,000,000";
			}
			for(int iLA =0;iLA<iword; iLA++){
				if(comboCount>2){
				
				enterCount = (1+iLA)%6; 
				winner +=iArray[iLA] + " ";
					if(enterCount==0){
						winner +="Have " + comboCount +" Same numbers, Wins " + strWin +" Pesos\n";
					}
				}
			}
			earnnings++;
		}
	}
	
	private static double wordcount(String str){
		boolean inWord = false;
		int len = str.length(),numWords = 1;
		for (int i = 0; i<len; i++) {
			char c = str.charAt(i);
			switch (c) {
			 case '\t':
			 case ' ':
				 if (inWord) {
					 numWords++;
					 inWord = false;
				 }
				 break;
			 default:
				 inWord = true;
			}
		} return numWords;
	}
	
	private static boolean isNumeric(String s) {
		  if (s == null || s.isEmpty()) return false;
		  for (int x = 0; x < s.length(); x++) {
		    char c = s.charAt(x);
		    if (x == 0 && (c == '-')) continue;
		    if ((c >= '0') && (c <= '9')) continue;
		    return false;
		  }
		  return true; 
	}
	
	public static int duplicatecheck (int [] arr){
		int duplicate=0;
			 for (int i = 0; i < arr.length; i++ ) {
		            for (int j = i + 1; j < arr.length; j++){
		            	if( arr[j]==arr[i] && arr[j]!=0 && arr[i] != 0 ){
		            		duplicate = 1;
		            	}
		            }
		     }
        return duplicate;
    }
	
	public static int combocheck(int [] arr,int [] arr2){
		int samecount=0;
		for(int i =0;i < arr.length;i++ ){
			for(int j = 0; j<arr2.length;j++){
				if(arr[i]==arr2[j]){
					samecount++;
				}
			}
        }return samecount;
	}
	
}

