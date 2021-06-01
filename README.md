# INTERNET_BANKING_PROJECT
Internet_banking_project using java concepts and array list implementation
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;
class BankApplication{
	public static void main(String[] args)
	{
		Scanner sc = new Scanner(System.in);
		List<BankAccount> bankAccountList = new ArrayList<>();
		String bankName= "ABI_internet_banking";
		showMenu(sc,bankName,bankAccountList);
		sc.close();
	}
	public static void showMenu(Scanner sc, String bankName, List<BankAccount> bankAccountList)
	{
		int option;
		BankAccount searchAccount;
		System.out.println("\n\n\t\t\t\t\tWELCOME TO "+bankName);

		System.out.println("\n");

		do
		{
			System.out.println("1:Registration\n");
			System.out.println("2:Account Details\n");
			System.out.println("3:Credit\n");
			System.out.println("4:Debit\n");
			System.out.println("5:Transfer Money\n");
			System.out.println("6:Transaction history\n");
			System.out.println("7:Exit The System\n");
			System.out.println("*******************");
			System.out.println("Enter Your Option:");
			System.out.println("*******************");
			option=sc.nextInt();
			sc.nextLine();
			System.out.println("\n");
			switch (option)
			{
			case 1:
				System.out.println("----------------------------------------------------------");
				BankAccount bankAccount = new BankAccount(bankName);
				bankAccount.registerAccount(sc);
				bankAccountList.add(bankAccount);
				System.out.println("----------------------------------------------------------");
				System.out.println("\n");
				break;
			case 2:
				String panNumber;
				System.out.println("----------------------------------------------------------");
				System.out.println("Enter the customers PAN card number ::");
				panNumber = sc.nextLine();
				searchAccount = searchAccount(bankAccountList, panNumber);
				if(searchAccount == null) {
					System.out.println("Account not found !!");
				}else {
					searchAccount.accountDetails();
				}
				System.out.println("----------------------------------------------------------");
				System.out.println("\n");
				break;
			case 3:
				System.out.println("----------------------------------------------------------");
				System.out.println("Enter the customers PAN card number ::");
				panNumber = sc.nextLine();
				searchAccount = searchAccount(bankAccountList, panNumber);
				if(searchAccount == null) {
					System.out.println("Account not found !!");
				}else {
					int creditAmount;
					System.out.println("----------------------------------------------------------");
					System.out.println("Enter an amount to credit:");
					System.out.println("----------------------------------------------------------");
					creditAmount=sc.nextInt();
					searchAccount.credit(creditAmount);
					System.out.println("\n");
				}
				break;
			case 4:
				System.out.println("----------------------------------------------------------");
				System.out.println("Enter the customers PAN card number ::");
				panNumber = sc.nextLine();
				searchAccount = searchAccount(bankAccountList, panNumber);
				if(searchAccount == null) {
					System.out.println("Account not found !!");
				}else {
					int debitAmount;
					System.out.println("----------------------------------------------------------");
					System.out.println("Enter an amount to debit:");
					System.out.println("----------------------------------------------------------");
					debitAmount=sc.nextInt();
					searchAccount.debit(debitAmount);
					System.out.println("\n");
				}
				break;
			case 5:
				System.out.println("----------------------------------------------------------");
				System.out.println("Enter the PAN card number of sender's account ::");
				panNumber = sc.nextLine();
				searchAccount = searchAccount(bankAccountList, panNumber);
				if(searchAccount == null) {
					System.out.println("Account not found !!");
				}else {
					System.out.println("----------------------------------------------------------");
					searchAccount.transfer(sc);
					System.out.println("----------------------------------------------------------");
					System.out.println("\n");
				}
				
				break;
			case 6:
				System.out.println("----------------------------------------------------------"); 
                                System.out.println("Enter the customers PAN card number ::");
				panNumber = sc.nextLine();
				searchAccount = searchAccount(bankAccountList, panNumber);
				if(searchAccount == null){
					System.out.println("Account not found !!");
                }else {
					
				System.out.println("----------------------------------------------------------");
				searchAccount.transactionHistory();
				System.out.println("----------------------------------------------------------");
				System.out.println("\n");
                 }
				break;
			case 7:
				System.out.println("==========================================================");
				System.out.println("\t\t\t\t\tTHANK YOU FOR USING OUR SEVICES!!!!!");
				break;
			default:
				System.out.println("Invalid Option!!!Please Enter Correct Option...");
				break;
			}
		}
		while(option!=7);
	}
	public static BankAccount searchAccount(List<BankAccount> bankAccountList, String panNumber) {
		
		for(int i=0; i<bankAccountList.size();i++) {
			BankAccount searchAccount = bankAccountList.get(i);
			if(searchAccount.panNumber.equals(panNumber)) {
				return searchAccount;
			}
		}
		return null;
	}
}
class BankAccount{
	String panNumber, customerIfscCode, bankName;
	String customerName,customerEmailId;
	long accountBalance,phoneNumber,accountNumber;
	int totalBalance;
	List<TransactionHistory> transactionHistoryList = new ArrayList<>();
	BankAccount(String bankName)
	{
		this.bankName=bankName;
	}
	void registerAccount(Scanner sc)
	{	
		System.out.print("Enter Account No: ");
		this.accountNumber = sc.nextLong();
		System.out.print("Enter Name: ");
		this.customerName = sc.next();
		sc.nextLine();
		System.out.print("Enter Your PAN card No.: ");
		this.panNumber = sc.nextLine();
		System.out.print("Enter Phone No.: ");
		this.phoneNumber = sc.nextLong();
		System.out.print("Enter Your Email-Id: ");
		this.customerEmailId = sc.next();
		sc.nextLine();
                System.out.println("Enter customer bank's IFSC code: ");
		customerIfscCode= sc.next();
                System.out.print("Enter Opening Balance: ");
		this.accountBalance = sc.nextLong();
		sc.nextLine();
		System.out.println("\n\n\t\t\t\t\tACCOUNT REGISTERED\n");
	}
	void accountDetails()
	{
		System.out.println("YOUR ACCOUNT No. :"+ this.accountNumber);
		System.out.println("BALANCE AMOUNT :" + this.accountBalance);
		System.out.println("NAME: "+ this.customerName);
		System.out.println("PAN CARD No.: " + this.panNumber);
		System.out.println("YOUR PHONE No.: "+ this.phoneNumber);
		System.out.println("YOUR EMAIL ID: "+ this.customerEmailId);

	}
	void credit(int creditAmount){
		if(creditAmount > 0){
			this.accountBalance=this.accountBalance+creditAmount;
			System.out.println(" YOUR CURRENT ACCOUNT BALANCE IS " + this.accountBalance);
			TransactionHistory transactionHistory = new TransactionHistory("CREDIT", creditAmount);
			transactionHistoryList.add(transactionHistory);
		}
		else {
			System.out.println("Invalid credit amount. It should be greater than 0.");
		}
	}
	void debit(int debitAmount)
	{
		if(debitAmount <= 0) {
			System.out.println("Invalid debit amount. It should be greater than 0.");
		}
		else if(this.accountBalance >= debitAmount)
		{
			this.accountBalance -= debitAmount;
			System.out.println(" YOUR CURRENT ACCOUNT BALANCE IS " + this.accountBalance);
			TransactionHistory transactionHistory = new TransactionHistory("DEBIT", debitAmount);
			transactionHistoryList.add(transactionHistory);
		}
		else
			System.out.println("INSUFFICIENT AMOUNT");
	}
	void transfer(Scanner sc)
	{
		String receiverName, receiverIfscCode;
		Long receiverAccountNumber;
		int transferAmount;
		System.out.println("ENTER THE RECEIPIENT'S ACCOUNT DETAILS:\n");
		System.out.println("Enter the receipient's account no.: ");
		receiverAccountNumber=sc.nextLong();
		System.out.println("Enter the receipient's name: ");
		receiverName= sc.next();
		sc.nextLine();
		System.out.println("Enter receiver bank's IFSC code: ");
		receiverIfscCode= sc.next();
		System.out.println("Enter the amount you want to transfer: ");
		transferAmount=sc.nextInt();
		if(this.accountBalance>totalBalance)
		{
			this.accountBalance=this.accountBalance-transferAmount;
			System.out.println("TRANSFER SUCCESSFUL TO ACCOUNT NUMBER: "+receiverAccountNumber + " receiverName: "+receiverName +" receiverIfscCode: "+receiverIfscCode);
		}
		else
			System.out.println("INSUFFICIENT FUND");
		System.out.println("Your Remaining balance is:"+ this.accountBalance);
		TransactionHistory transactionHistory = new TransactionHistory("TRANSFER", transferAmount);
		transactionHistoryList.add(transactionHistory);
	}
	void transactionHistory()
	{
		if(this.transactionHistoryList.size()>0)
		{
			for(int i=0;i<transactionHistoryList.size();i++) {
				System.out.println("Transaction Type = "+transactionHistoryList.get(i).paymentType + " Trnsaction Amount = "+transactionHistoryList.get(i).paymentAmount);
			}
		}
		else{
			System.out.println("No Transaction Occured");

		}
	}
}
class TransactionHistory{
	String paymentType;
	int paymentAmount;
	
	public TransactionHistory(String paymentType, int paymentAmount) {
		this.paymentType = paymentType;
		this.paymentAmount = paymentAmount;
	}
}
