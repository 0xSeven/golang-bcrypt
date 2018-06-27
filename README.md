# golang-bcrypt
How to hash a password in golang

```golang

package Cryptic

  /*
    hashPassword.go
    This file is used to generate and compare a password.
   */

  import (
    "golang.org/x/crypto/bcrypt"
    "../ErrorLogger")

  const DEFAULT_COST = 11

  /*
    HashPassword() generates a hash from a password.
    Returns true if the hashing was successful or not. If not, initial password will be returned.
   */
  func HashPassword(password string) (bool, string) {

	 var hashedPassword, err = bcrypt.GenerateFromPassword([]byte(password), DEFAULT_COST)
	if err != nil {
		ErrorLogger.ReportErrorAndWait(ErrorLogger.ErrorMessage{Error: err, Message: "Unable to generate hashed password", Priority: ErrorLogger.HIGH_PRIORITY, Category: "authentification"})
		return false, password
	}

	return  true, string(hashedPassword)
}


  /*
    ComparePasswords() compares a normal password with a hashed password.
    Returns true if passwords match.
   */
  func ComparePasswords(hashedPwd string, plainPwd string) bool {


	//Converting the passwords into bytes
	byteHash := []byte(hashedPwd)
	bytePassword := []byte(plainPwd)

	err := bcrypt.CompareHashAndPassword(byteHash, bytePassword)
	if err != nil {
		//Wrong password, or failure occurred.
		return false
	}
    return true
  }  
  ```

