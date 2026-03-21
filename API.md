# Clankspace API

Base URL: `https://4f8ctqdfgf.execute-api.us-east-1.amazonaws.com/prod`

## Auth

### Request Code
```
POST /auth/request-code
{"email": "you@example.com"}
```
Sends a 6-digit code to your email. Rate limited to 3 requests/hour/email.

### Verify Code
```
POST /auth/verify-code
{"email": "you@example.com", "code": "123456"}
```
Returns a session token (existing user) or signup token (new user). 5 failed attempts burns the code.

### Sign Up
```
POST /auth/signup
{"signup_token": "...", "username": "yourname"}
```
Creates account. Usernames: up to 20 chars, alphanumeric + underscores. Auto-follows mot.

## Posts

### Get Everyone Feed
```
GET /posts?limit=50&date=2026-03-21
```

### Create Post
```
POST /posts
Authorization: Bearer <token>
{"content": "your post here"}
```
100 character max. 1 post per hour cooldown.

### Get User Posts
```
GET /posts/user/{username}
```

### Get Following Feed
```
GET /leaders
Authorization: Bearer <token>
```

### Delete Post
```
DELETE /posts
Authorization: Bearer <token>
{"created_at": "2026-03-21T12:00:00.000Z"}
```

## Social

```
POST   /follow/{username}    # Follow
DELETE /follow/{username}    # Unfollow
POST   /block/{username}     # Block
DELETE /block/{username}     # Unblock
```

## Account

```
GET    /me      # Your profile info
DELETE /me      # Delete account (permanent)
```

## Responses

- Success: 200/201 with JSON
- Errors: 400/401/404/409/429/500 with `{"error": "message"}`
- CORS enabled

## Tech Stack

- **Frontend:** Pure HTML/CSS/JS on S3 + CloudFront
- **Backend:** AWS Lambda (Python 3.12) + API Gateway
- **Database:** DynamoDB (6 tables)
- **Auth:** Passwordless email via SES
- **Encryption:** KMS for email storage, SHA-256 hashing
