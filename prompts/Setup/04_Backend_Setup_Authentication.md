# 04_Backend_Setup_Authentication.md

**Purpose:**
This file guides the AI Agent to implement JWT-based authentication in the NestJS backend. This includes installing necessary packages, creating authentication modules, services, controllers, and implementing login and user registration functionality.

**Instructions:**

1.  **Install JWT and Passport packages:**
    *   Install the `@nestjs/jwt` and `@nestjs/passport` packages for JWT authentication and Passport integration within NestJS. Also install `passport` and `@types/passport`.

    ```bash
    npm install @nestjs/jwt @nestjs/passport passport @types/passport
    ```

2.  **Create Authentication Module:**
    *   Use the NestJS CLI to generate an `auth` module:

    ```bash
    nest g module auth
    ```

3.  **Create Authentication Service:**
    *   Within the `src/auth` folder, create a service named `auth.service.ts`:

    ```bash
    nest g service auth --no-spec
    ```

    *   Implement the following methods in `auth.service.ts`:
        *   `register(userDto)`:  Handles user registration.  For now, a basic implementation that creates a user (we'll define the User entity/model later in more detail, for now assume a simple `User` model with `email` and `password` properties). Password hashing should be implemented using a library like `bcrypt` (install if needed: `npm install bcrypt @types/bcrypt`).
        *   `login(userDto)`:  Handles user login. Validates user credentials against the database, and upon successful authentication, generates a JWT token.
        *   `validateUser(email, password)`:  Verifies user credentials against the database. This method will be used by Passport strategies.

    *   Example `auth.service.ts` (basic structure - implementation details will be refined later, especially password handling and user entity):

    ```typescript  typescript
    // src/auth/auth.service.ts
    import { Injectable, UnauthorizedException } from '@nestjs/common';
    import { JwtService } from '@nestjs/jwt';
    // import * as bcrypt from 'bcrypt'; // Install bcrypt if needed

    @Injectable()
    export class AuthService {
      constructor(private jwtService: JwtService) {}

      async register(userDto: any): Promise<any> {
        // TODO: Implement user registration logic (save to database, hash password)
        // For now, just return a placeholder
        return { message: 'User registered successfully' };
      }

      async validateUser(email: string, pass: string): Promise<any> {
        // TODO: Implement user validation against database
        // For now, placeholder user validation
        const user = { email: '[email address removed]', password: 'password123' }; // Replace with DB lookup later
        if (user && user.email === email && user.password === pass) { // Insecure password check - replace with bcrypt
          return user;
        }
        return null;
      }

      async login(user: any) {
        const payload = { email: user.email, sub: user.userId }; // sub (subject) is often user ID
        return {
          access_token: await this.jwtService.signAsync(payload),
        };
      }
    }
    ```

4.  **Create Authentication Controller:**
    *   Within the `src/auth` folder, create a controller named `auth.controller.ts`:

    ```bash
    nest g controller auth --no-spec
    ```

    *   Implement the following endpoints in `auth.controller.ts`:
        *   `register(userDto)`:  Endpoint for user registration, calling `authService.register()`.
        *   `login(userDto)`: Endpoint for user login, calling `authService.login()`.

    *   Example `auth.controller.ts` (basic structure):

    ```typescript  typescript
    // src/auth/auth.controller.ts
    import { Controller, Post, Body } from '@nestjs/common';
    import { AuthService } from './auth.service';

    @Controller('auth')
    export class AuthController {
      constructor(private authService: AuthService) {}

      @Post('register')
      async register(@Body() userDto: any) { // Define User DTO later
        return this.authService.register(userDto);
      }

      @Post('login')
      async login(@Body() userDto: any) { // Define Login DTO later
        return this.authService.login(userDto);
      }
    }
    ```

5.  **Create JWT Strategy (Passport):**
    *   Within the `src/auth` folder, create a folder named `strategies`.
    *   Inside `src/auth/strategies`, create a file named `jwt.strategy.ts`:

    ```bash
    mkdir src/auth/strategies
    touch src/auth/strategies/jwt.strategy.ts
    ```

    *   Implement the JWT strategy in `jwt.strategy.ts` using `@nestjs/passport` and `passport-jwt`. This strategy will be responsible for verifying JWT tokens and extracting user information from the payload.

    ```typescript  typescript
    // src/auth/strategies/jwt.strategy.ts
    import { Injectable } from '@nestjs/common';
    import { PassportStrategy } from '@nestjs/passport';
    import { ExtractJwt, Strategy } from 'passport-jwt';

    @Injectable()
    export class JwtStrategy extends PassportStrategy(Strategy) {
      constructor() {
        super({
          jwtFromRequest: ExtractJwt.fromAuthHeaderAsBearerToken(),
          ignoreExpiration: false, // Let Passport handle expiration
          secretOrKey: 'YOUR_SECRET_KEY', // TODO: Move to environment variable and secure place!
        });
      }

      async validate(payload: any) {
        // Payload will contain the information from the JWT (e.g., email, sub)
        // Here you can optionally fetch user from database based on payload.sub (userId)
        // and augment the request object with the user.
        return { userId: payload.sub, email: payload.email }; // Return user info to be added to request.user
      }
    }
    ```
    *   **Important:** Replace `'YOUR_SECRET_KEY'` with a strong, secret key. **This MUST be moved to an environment variable (`.env` file) and securely managed in a production environment.** For now, for development, a simple string will suffice, but remember to change this later.

6.  **Configure JWT Module in Auth Module:**
    *   In `src/auth/auth.module.ts`, configure the `JwtModule` and import necessary modules.

    ```typescript  typescript
    // src/auth/auth.module.ts
    import { Module } from '@nestjs/common';
    import { AuthService } from './auth.service';
    import { AuthController } from './auth.controller';
    import { JwtModule } from '@nestjs/jwt';
    import { JwtStrategy } from './strategies/jwt.strategy';
    import { PassportModule } from '@nestjs/passport';

    @Module({
      imports: [
        PassportModule,
        JwtModule.register({
          secret: 'YOUR_SECRET_KEY', // TODO: Move to environment variable!
          signOptions: { expiresIn: '60s' }, // Example: Token expires in 60 seconds (adjust as needed)
        }),
      ],
      controllers: [AuthController],
      providers: [AuthService, JwtStrategy],
    })
    export class AuthModule {}
    ```
    *   **Important:** Again, replace `'YOUR_SECRET_KEY'` with a strong, secret key and move it to environment variables. Adjust `expiresIn` as needed for token expiration.

7.  **Import Auth Module into AppModule:**
    *   Import the `AuthModule` into your `AppModule` (`src/app.module.ts`) to make the authentication features available.

    ```typescript  typescript
    // src/app.module.ts
    import { Module } from '@nestjs/common';
    import { AppController } from './app.controller';
    import { AppService } from './app.service';
    import { CommonModule } from './common/common.module';
    import { AuthModule } from './auth/auth.module'; // Import AuthModule

    @Module({
      imports: [CommonModule, AuthModule], // Add AuthModule to imports
      controllers: [AppController],
      providers: [AppService],
    })
    export class AppModule {}
    ```

**Context:**

*   We are implementing JWT-based authentication to secure our backend API.
*   This will allow us to protect endpoints and identify authenticated users.
*   We are using `@nestjs/jwt` and `@nestjs/passport` to simplify JWT integration in NestJS.

**Rules and Constraints:**

*   Implement JWT-based authentication.
*   Use `@nestjs/jwt` and `@nestjs/passport` packages.
*   Create an `AuthModule` to encapsulate authentication logic.
*   Implement `AuthService`, `AuthController`, and `JwtStrategy`.
*   Use JWT for token generation and verification.
*   For now, use a placeholder secret key (`'YOUR_SECRET_KEY'`) but remember to **replace it with a strong, environment variable based secret in `.env` for security.**

**Details:**

*   JWT Secret Key:  `'YOUR_SECRET_KEY'` (placeholder - **MUST BE REPLACED and moved to environment variable**)
*   Token Expiration: `'60s'` (example, adjust as needed in `JwtModule.register()` options).
*   Authentication Module: `src/auth`
*   JWT Strategy: `src/auth/strategies/jwt.strategy.ts`

**Verification:**

*   After executing these instructions, the AI Agent should confirm:
    *   Installation of `@nestjs/jwt`, `@nestjs/passport`, `passport`, `@types/passport` npm packages.
    *   Creation of `AuthModule` in `src/auth` folder.
    *   Creation of `AuthService` and `AuthController` in `src/auth`.
    *   Creation of `JwtStrategy` in `src/auth/strategies`.
    *   Implementation of basic `register`, `login`, and `validateUser` methods in `AuthService` (placeholder implementations are acceptable for now).
    *   Implementation of `register` and `login` endpoints in `AuthController`.
    *   Configuration of `JwtModule` and `PassportModule` in `AuthModule`.
    *   Import of `AuthModule` into `AppModule`.

**Next Steps (for AI Agent):**

1.  Execute the commands and steps outlined in the "Instructions" section.
2.  Verify each step as described in the "Verification" section.
3.  Provide a summary of actions taken and confirmation of successful Authentication setup.
4.  Once confirmed, proceed to the next file: `05_Backend_Setup_MultiTenancy.md`.