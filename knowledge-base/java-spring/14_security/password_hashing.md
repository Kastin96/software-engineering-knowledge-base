# Password Hashing

If the application stores local user passwords, never store plaintext passwords.

Use a password encoder designed for password hashing.

## BCrypt Example

```java
@Bean
PasswordEncoder passwordEncoder() {
    return new BCryptPasswordEncoder();
}
```

## Registration Flow

```java
String passwordHash = passwordEncoder.encode(command.rawPassword());
userRepository.save(new UserEntity(command.email(), passwordHash));
```

## Login Flow

```java
boolean matches = passwordEncoder.matches(rawPassword, user.passwordHash());
```

## Important Notes

- Do not use general-purpose hashes such as plain SHA-256 for passwords.
- Do not log raw passwords.
- Do not return password hashes from APIs.
- Prefer external identity providers when the product does not need to own
  password management.

## Key Idea

Password storage is a high-risk responsibility. If you own it, use a strong
password encoder and keep hashes away from API responses and logs.
