# Register_Login


### Class User

```Ruby
class User {
  constructor(userData) {
    this.userData = { ...userData };
  }

  save() {
    // logic save data
  }

  checkExistUser() {
    return new Promise((resolve, reject) => {
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

  static Login(userData) {
    return new Promise((resolve, reject) => {
      dbConnect('users', async (db) => {
        try {
          const user = await db.findOne(
            { email: userData.email },
            { projection: { email: 1, password: 1, username: 1 } },
          );

          if (
            !user || !compareSync(userData.password, user.password)
          ) {
            return reject(
              createError.Unauthorized(
                'Please enter valid email and password',
              ),
            );
          }
          resolve(user);
        } catch (error) {
          return reject(error);
        }
      });
    });
  }
}
```
