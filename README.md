object ATM {

  // Simulated database of user accounts with their associated fingerprint data and balance
  case class UserAccount(accountNumber: String, fingerprintData: String, balance: Double)

  val userAccounts = List(
    UserAccount("123456789", "fingerprint_hash_1", 1000.00),
    UserAccount("987654321", "fingerprint_hash_2", 500.00)
  )

  // Function to simulate fingerprint scanning and matching
  def scanFingerprint(): String = {
    // In a real scenario, this would interface with the fingerprint hardware and return the scanned fingerprint data
    // Here, we will simulate with a simple prompt
    println("Please place your thumb on the scanner...")
    val scannedFingerprint = scala.io.StdIn.readLine("Enter simulated fingerprint data: ")
    scannedFingerprint
  }

  // Function to find user account by fingerprint
  def findAccountByFingerprint(fingerprintData: String): Option[UserAccount] = {
    userAccounts.find(account => account.fingerprintData == fingerprintData)
  }

  // Function to withdraw money
  def withdrawMoney(account: UserAccount, amount: Double): Either[String, UserAccount] = {
    if (amount <= 0) {
      Left("Invalid withdrawal amount. Please enter a positive number.")
    } else if (amount > account.balance) {
      Left("Insufficient balance.")
    } else {
      val updatedAccount = account.copy(balance = account.balance - amount)
      Right(updatedAccount)
    }
  }

  // Main function to simulate the withdrawal process
  def main(args: Array[String]): Unit = {
    // Step 1: Scan the fingerprint
    val scannedFingerprint = scanFingerprint()

    // Step 2: Find the corresponding user account
    findAccountByFingerprint(scannedFingerprint) match {
      case Some(account) =>
        println(s"Welcome, account number ${account.accountNumber}!")

        // Step 3: Enter the amount to withdraw
        val amount = scala.io.StdIn.readLine("Enter the amount you want to withdraw: ").toDouble

        // Step 4: Attempt to withdraw the money
        withdrawMoney(account, amount) match {
          case Right(updatedAccount) =>
            println(s"Withdrawal successful! Your new balance is ${updatedAccount.balance}")
          case Left(error) =>
            println(s"Error: $error")
        }

      case None =>
        println("Fingerprint not recognized. Please try again.")
    }
  }
}
