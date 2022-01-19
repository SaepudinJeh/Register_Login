# Register_Login


### Class User

```Ruby
class User {
  
  # Contructor Class
  constructor(userData) {
    this.userData = { ...userData };
  }
  
  # Save User Register
  save() {
    # logic save data
  }
  
  # Check User Exist
  checkExistUser() {
    # Logic Check User Exist 
  }
  
  # Check User from Login
  static Login() {
    # Logic Login User
  }
}
```

### Method Save User
```Ruby
save() {
    return new Promise((resolve, reject) => {
      try {
        dbConnect('users', async (db) => {
          
          # Change lower case Email before save databases
          const lowerEmail = this.userData.email.toLowerCase();
          this.userData.email = lowerEmail;
          
          # Encrypt Password before save to Databases
          const hashedPassword = hashSync(this.userData.password, 11);
          this.userData.password = hashedPassword;
          
          # Save Data User to Databases
          const Place = await db.insertOne(this.userData);

          resolve(Place);
        });
      } catch (error) {
        return reject(error);
      }
    });
  }
```


### Method Check Existing User
```Ruby
checkExistUser() {
    return new Promise((resolve, reject) => {
      #
      dbConnect('users', async (db) => {
        try {
          const emailUser = this.userData.email;

          const user = await db.findOne({
            email: emailUser,
          });

          if (!user) {
            resolve({
              check: false,
            });
          } else if (emailUser === user.email) {
            resolve({
              check: true,
              message: 'This email is already in use',
            });
          }
        } catch (error) {
          return reject(error);
        }
      });
    });
  }
```
