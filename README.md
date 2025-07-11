# Email Verifier

This is a Go application that provides a robust and efficient solution for email verification. It can be run as a standalone service, offering both single and batch email verification through a simple API.

## Features

- **Single Email Verification**: Quickly verify a single email address.
- **Batch Email Verification**: Verify a list of email addresses in a single request, with real-time progress updates using Server-Sent Events (SSE).
- **Comprehensive Checks**: Performs various checks, including syntax validation, DNS MX record lookup, and SMTP verification.
- **Easy to Deploy**: Can be easily run as a standalone server.

## Getting Started

### Prerequisites

- [Go](https://golang.org/doc/install) (version 1.15 or higher)
- [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

### Installation and Setup

1.  **Clone the repository:**

    ```bash
    git clone https://github.com/kamesh27/Email-Verifier.git
    cd Email-Verifier
    ```

2.  **Install dependencies:**

    ```bash
    go mod tidy
    ```

3.  **Run the server:**

    ```bash
    go run cmd/apiserver/main.go
    ```

    The server will start on `http://localhost:8080`.

## API Usage

### Single Email Verification

Send a `GET` request to verify a single email address.

- **Endpoint**: `/v1/{email}/verification`
- **Method**: `GET`
- **Example**:

  ```bash
  curl http://localhost:8080/v1/example@gmail.com/verification
  ```

### Batch Email Verification

To verify multiple emails, first initiate a batch job, and then connect to the stream to receive real-time results.

#### 1. Initiate Batch Job

Send a `POST` request with a JSON array of emails to start the verification process.

- **Endpoint**: `/batch/initiate`
- **Method**: `POST`
- **Body**:

  ```json
  {
    "emails": ["test1@example.com", "test2@gmail.com", "invalid-email"]
  }
  ```

- **Example**:

  ```bash
  curl -X POST -H "Content-Type: application/json" -d '{"emails": ["test1@example.com", "test2@gmail.com"]}' http://localhost:8080/batch/initiate
  ```

  The response will contain a `jobId` that you will use in the next step.

  ```json
  {
    "jobId": "your-unique-job-id"
  }
  ```

#### 2. Stream Verification Results

Connect to the SSE stream using the `jobId` to get real-time updates for each email.

- **Endpoint**: `/batch/stream/{jobID}`
- **Method**: `GET`
- **Example**:

  ```bash
  curl http://localhost:8080/batch/stream/your-unique-job-id
  ```

  The server will stream back results as they become available.

## Contributing

Contributions are welcome! Please feel free to submit a pull request.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
