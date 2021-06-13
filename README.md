# Register-From//Import the Package:
import java.sql.*;
import java.util.Scanner;

public class Admin_Roll {
	      static 	String 	USER_ID ,PASSWORD;
	public static void main(String[] args)
	{
		try {
		// Load and Register the driver:
		Class.forName("oracle.jdbc.driver.OracleDriver");
		
		
		//Establish the connection:
		Connection con = DriverManager.getConnection("jdbc:oracle:thin:@localhost:1521:xe","XE","2808");
		

		String FIRST_NAME,LAST_NAME,PHONE_NUMBER,GENDER,CITY,STATE,ROLL;
		String result_1,result_2,ACCESS = "", Roll = "Admin";
		int POSTAL_CODE ,Option ,Choice;
		
		Scanner scan =  new Scanner(System.in);
		System.out.println("Enter your choice : 1 for SignUp , 2 for SignIn, 3 for Update:");
		Option = scan.nextInt();
		
		Choice =Option ;
		
		switch(Choice) 
		{
		
		case 1:
			System.out.println("Register");
		
		System.out.println("Enter the first_name:");
		FIRST_NAME = scan.next();
		
		System.out.println("Enter the Last_name:");
		LAST_NAME = scan.next();
		
		System.out.println("Create yout  user_ID:");
		USER_ID = scan.next();
		

		System.out.println("Create your Password:");
		PASSWORD = scan.next();
		
		System.out.println("Enter the Mobile_number");
		PHONE_NUMBER = scan.next();
		
		System.out.println("Enter the Gender");
		GENDER = scan.next();
		
		System.out.println("Enter the date of birth");
		String doj = scan.next();
		
		System.out.println("Enter the City:");
		CITY= scan.next();
		
		System.out.println("Enter the State:");
	 	STATE = scan.next();
	 	
	 	System.out.println("Enter the Postal_code:");
		POSTAL_CODE = scan.nextInt();
		
		
		java.sql.Date sqdoj = java.sql.Date.valueOf(doj);
		

		String sql = "INSERT INTO  Signup VALUES(?,?,?,?,?,?,?,?,?,?)";
		// Create the statement and execute the qery
		PreparedStatement ps = con.prepareStatement(sql);
		ps.setString(1,FIRST_NAME );
		ps.setString(2,LAST_NAME);
		ps.setString(3,USER_ID);
		ps.setString(4,PASSWORD);
		ps.setString(5,PHONE_NUMBER); 
		ps.setString(6,GENDER);
		ps.setDate(7,sqdoj);
		ps.setString(8,CITY);
		ps.setString(9,STATE);
		ps.setInt(10,POSTAL_CODE);
		
		//Result set
				int count = ps.executeUpdate();
				System.out.println("row inserted");
				break;
		case 2:
		
			System.out.println("Login");
			
			System.out.println("Enter your Vaild User_id:");
			 USER_ID = scan.next();
			
			System.out.println("Enter your Vaild  password :");
			PASSWORD = scan.next();
			
			System.out.println("Enter your Admin_Roll :");
			ROLL = scan.next();
			
			
			String sql2 = "INSERT INTO SignIn Values(?,?,?)";
			
			PreparedStatement ps1 = con.prepareStatement(sql2);
			ps1.setString(1,USER_ID);
			ps1.setString(2,PASSWORD);
			ps1.setString(3,ROLL);
			
			int count1 = ps1.executeUpdate() ;
			System.out.println("YOUR LOGIN SUCESSFULLY");
			break;
			
			
		case 3:
			// TAKE ROLL FROM USER_NAME
			System.out.println("Update :");
			System.out.println("Enter your Valid user_id:");
			USER_ID = scan.next();
			
			
			String qery = "SELECT ROLL FROM SIGNIN WHERE USER_ID ='"+USER_ID+"'";
			Statement ST = con.createStatement();
			ResultSet RS = ST.executeQuery(qery);
			RS.next();
			 result_1 = RS.getString(1);
			
			
			//TAKE ROLL FROM PASSWORD 
			System.out.println("Enter your Password:");
			PASSWORD = scan.next();
			
			String QUERY = "SELECT ROLL FROM SIGNIN WHERE PASSWORD ='"+PASSWORD+"'";
			Statement st = con.createStatement();
			ResultSet rs = st.executeQuery(QUERY);
			rs.next();
			 result_2 = rs.getString(1);
			  
		
			if(result_1.equals(result_2)) 
			{
				ACCESS = result_1;
				System.out.println("Type of roll get: "+ACCESS );
			}
			if(Roll.equals(ACCESS))
			{
				System.out.println("Permission Grand to access the data !");
				
				
				System.out.println("Choose 1 for Update & Chosse 2 for delete ");
				
				System.out.println("Enter the your Chosse");
				int chosse = scan.nextInt();
				
				if (chosse == 1) 
				{
					System.out.println("Update the data");
					
					System.out.println("Update the mobile_number:");
					String new_MOBILE_NUMBER = scan.next();
					
					System.out.println("Enter your valid USER_id:");
					USER_ID= scan.next();
					
					String UPDATE = ("UPDATE SignUp SET PHONE_NUMBER =? WHERE USER_ID =?");
					
					PreparedStatement PST = con.prepareStatement(UPDATE);
					PST.setString(1,new_MOBILE_NUMBER);
					PST.setString(2,USER_ID );
					int count2 = PST.executeUpdate();
					
					System.out.println("SUCESSFULLY DATA UPDATED");
					break;
					
				}	

				else if (chosse == 2) 
				{
					System.out.println("Delete your records");
					//CHILD TABLE
					System.out.println("Enter the user_id");
					USER_ID= scan.next();
					
					String sql4 ="DELETE FROM SIGNIN WHERE USER_ID ='"+USER_ID +"'";
					Statement str = con.createStatement();
					int count2 = str.executeUpdate(sql4);
				
					//PARENT TABLE 
					String sql1 ="DELETE FROM SIGNUP WHERE USER_ID ='"+USER_ID+"'";
					Statement st1 = con.createStatement();
					int count6 = st1.executeUpdate(sql1);
					System.out.println(count6);
					
					System.out.println("Your data  is deleted");
					break;
					
				}	
				}
			default:
				System.out.println("your are not admin ! admin only access the data");
		}
		}
		catch(Exception e)
		{
			System.out.println(e);
		}

	}
}
