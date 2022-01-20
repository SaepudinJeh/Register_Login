# Register_Login


## Class User

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
  Login() {
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
          
          # Return if successful
          resolve(Place);
        });
      } catch (error) {
        
        # Return if it fails
        return reject(error);
      }
    });
  }
```


### Method Check Existing User
```Ruby
checkExistUser() {
    return new Promise((resolve, reject) => {
      # using "users" tables
      dbConnect('users', async (db) => {
        try {
          # Get payload Email
          const emailUser = this.userData.email;
          
          # Check users in databases based on email
          const user = await db.findOne({
            email: emailUser,
          });
          
          # If the user does not exist then return false, if there is already a true return
          if (!user) {
            
            # Return if successful
            resolve({
              check: false,
            });
          } else if (emailUser === user.email) {
            # return result
            resolve({
              check: true,
              message: 'This email is already in use',
            });
          }
        } catch (error) {
        
          # Return if it fails
          return reject(error);
        }
      });
    });
  }
```

### Method Login User

```Ruby
Login() {
    return new Promise((resolve, reject) => {
      dbConnect('users', async (db) => {
        try {
          const { email, password } = this.userData;
          
          # Search by email and release email results, username, password
          const user = await db.findOne(
            { email: userData.email },
            { projection: { email: 1, password: 1, username: 1 } },
          );
          
          # Logic Compare passwords
          if (
            !user || !compareSync(this.userData.password, user.password)
          ) {
            return reject(
              createError.Unauthorized(
                'Please enter valid email and password',
              ),
            );
          }
          
          # Return result
          resolve(user);
        } catch (error) {
        
          # Return if it fails
          return reject(error);
        }
      });
    });
  }
```
