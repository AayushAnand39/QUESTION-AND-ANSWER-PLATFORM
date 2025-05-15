# QUESTION AND ANSWER PLATFORM

Welcome to the **QUESTION AND ANSWER PLATFORM** repository! This platform is developed entirely in the **C programming language** and serves as a robust system for managing question-and-answer interactions. The repository is designed to be **efficient**, **scalable**, and **easy to extend**.

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Usage](#usage)
- [Directory Structure](#directory-structure)
- [Contributing](#contributing)
- [License](#license)
- [Acknowledgements](#acknowledgements)

## Overview

The **QUESTION AND ANSWER PLATFORM** is a lightweight and efficient solution for handling Q&A functionalities. It is implemented entirely in the **C programming language**, providing high performance and portability. 

This platform is suitable for:
- **Educational purposes**: Facilitating knowledge-sharing in academic environments.
- **Community-driven Q&A forums**: Enabling users to ask and answer questions in an organized manner.
- **Technical support platforms**: Providing a structured way to handle user queries and responses.

## Features

- **Question Management**: Add, delete, and manage questions.
- **Answer Management**: Submit and view answers for questions.
- **Search Functionality**: Search for questions or answers based on keywords.
- **User Management**: Manage user profiles and activity (optional, if implemented in the future).
- **Lightweight**: Implemented in pure C for performance and efficiency.
- **Customizable**: Designed to be extended with additional features.

## Prerequisites

To build and run this platform, you will need:

1. A **C compiler** (e.g., `gcc`).
2. A **terminal** or command-line interface.
3. Basic knowledge of **C programming** and Makefiles (if using one).

## Usage

1. **Launching the Platform**:
   Run the compiled executable to start the platform.

2. **Adding Questions**:
   Follow the on-screen instructions to add new questions.

3. **Answering Questions**:
   Select a question and provide answers as prompted.

4. **Searching**:
   Use the search feature to look for specific questions or answers.

5. **Exiting**:
   Use the appropriate menu option to exit the platform safely.

## Features of `client.c`

The `client.c` file implements the client-side functionality for the **Question and Answer Platform**, enabling users to interact with the server using a menu-driven interface. Below are the key features:

### General Features
- **Socket Connection**: Establishes a TCP connection to the server using sockets.
- **Menu-Driven Interface**: Displays a dynamic menu based on the user's authentication status.

### User Authentication
- **Register**: Allows new users to register by sending their credentials to the server.
- **Login**: Authenticates existing users and provides access to additional features.

### Question and Answer Management
- **Post a Question**: Users can post new questions to the platform.
- **List Questions**: Retrieves and displays a list of all available questions with details (e.g., author, number of answers).
- **Answer a Question**: Enables users to submit answers to specific questions.
- **Search Questions**: Allows users to search for specific questions and view their details, including answers.

### Interactive Features
- **Rate an Answer**: Users can rate answers to a question on a scale of 1 to 5.
- **View Leaderboard**: Displays the leaderboard showing top contributors based on ratings or activity.

### Additional Functionalities
- **Dynamic Input Handling**: Processes user input for various operations.
- **Data Parsing and Display**: Parses server responses and displays data in a user-friendly format (e.g., questions, answers, leaderboard).
- **Error Handling**: Handles invalid inputs and server errors gracefully.
- **Exit**: Safely terminates the connection and exits the program.

This file is an essential part of the platform, providing a seamless interface for users to interact with the server and manage the Q&A system.


## Commands and Structures in `client.c`

The `client.c` file is the client-side implementation of the **Question and Answer Platform**. Below is a detailed explanation of its commands and structures:

---

### **Header Files**
- **`#include <stdio.h>`**  
  Provides functions for input and output operations such as `printf`, `scanf`, and `fgets`.

- **`#include <stdlib.h>`**  
  Provides utility functions like `exit()` and memory management functions.

- **`#include <string.h>`**  
  Used for string manipulation functions like `strcpy`, `strtok`, and `strcat`.

- **`#include <unistd.h>`**  
  Provides POSIX API functions such as `close()` for closing the socket connection.

- **`#include <arpa/inet.h>`**  
  Defines structures and functions for internet operations like `inet_pton` and `htons`.

- **`#include <sys/socket.h>`**  
  Used to create and manage sockets for communication.

---

### **Macros**
- **`#define PORT 8080`**  
  Specifies the port number used to connect to the server.

- **`#define BUFFER_SIZE 2048`**  
  Defines the size of the buffer used for sending and receiving data between the client and server.

---

### **Functions**

#### **`print_menu(int authenticated)`**
Displays a menu based on the user's authentication status:
- **Unauthenticated Users**: Options for registration and login.
- **Authenticated Users**: Options to post questions, answer questions, search, rate answers, view leaderboard, and exit.

#### **`display_questions(const char *buffer)`**
Parses and displays a list of questions retrieved from the server:
1. Skips the response status (e.g., `OK|`) in the server's response.
2. Tokenizes the data payload using `strtok` with `;` as the delimiter.
3. Extracts question details (ID, question text, author, and answer count).
4. Prints the parsed questions in a user-friendly format.

#### **`display_search_results(const char *buffer)`**
Displays search results for a specific query:
1. Parses the response into distinct parts:
   - Status (`OK` or `ERROR`).
   - Question text or "Question not found".
   - Answers or "No answers yet".
2. If the question is found, tokenizes and displays the answers.

---

### **Socket and Communication**

#### **Socket Creation**
- **`int sock = socket(AF_INET, SOCK_STREAM, 0);`**  
  Creates a TCP socket:
  - `AF_INET`: Specifies IPv4 addressing.
  - `SOCK_STREAM`: Indicates a TCP socket.

#### **Server Address Setup**
- **`struct sockaddr_in serv_addr;`**  
  A structure holding the server's address details:
  - `sin_family`: Specifies the address family (IPv4).
  - `sin_port`: Sets the port number using `htons(PORT)`.
  - `sin_addr`: Converts the IP address from text to binary using `inet_pton`.

#### **Connection Establishment**
- **`connect(sock, (struct sockaddr *)&serv_addr, sizeof(serv_addr));`**  
  Establishes a connection to the server using the socket.

---

### **Menu Options**

#### **1. Register**
- Prompts the user for a username and password.
- Sends the formatted command `REGISTER|username|password` to the server.
- Displays the server's response.

#### **2. Login**
- Prompts the user for a username and password.
- Sends the formatted command `LOGIN|username|password` to the server.
- Parses the server's response to determine if login is successful.

#### **3. Post a Question**
- Prompts the user to enter a question.
- Sends the formatted command `POST|question` to the server.
- Displays the server's response.

#### **4. List Questions**
- Sends the `LISTQ` command to the server.
- Parses and displays the list of questions using `display_questions`.

#### **5. Answer a Question**
- Prompts the user for a question number and their answer.
- Sends the formatted command `ANSWER|question_number|answer` to the server.
- Displays the server's response.

#### **6. Search Questions**
- Prompts the user for a search query.
- Sends the formatted command `SEARCH|query` to the server.
- Parses and displays the search results using `display_search_results`.

#### **7. Rate an Answer**
- Prompts the user for the question ID, answer number, and rating.
- Sends the formatted command `RATE|question_id|answer_number|rating` to the server.
- Displays the server's response.

#### **8. View Leaderboard**
- Sends the `LEADER` command to the server.
- Parses and displays the leaderboard.

#### **9. Exit**
- Closes the socket connection using `close(sock)`.
- Exits the program using `exit(0)`.

---

### **String and Buffer Handling**

#### **String Manipulation**
- **`snprintf`**: Safely formats strings to construct commands like `REGISTER`, `LOGIN`, `POST`, etc.
- **`strtok`**: Splits strings into tokens based on a delimiter. Used to parse server responses.
- **`strchr`**: Finds the first occurrence of a character in a string. Used to skip headers in server responses.

#### **Buffer Management**
- **`char buffer[BUFFER_SIZE] = {0};`**: Initializes the buffer with zeros to avoid garbage data.
- **`memset(buffer, 0, BUFFER_SIZE);`**: Clears the buffer after each interaction.

---

### **Network Communication**

#### **Sending Data**
- **`send(sock, buffer, strlen(buffer), 0);`**  
  Sends data from the client to the server.

#### **Receiving Data**
- **`recv(sock, buffer, BUFFER_SIZE, 0);`**  
  Receives data from the server into the buffer.

---

### **Error Handling**
- Checks for invalid inputs and server errors.
- Displays appropriate error messages if the connection fails or an invalid menu option is selected.

---

This detailed explanation covers all commands and structures used in `client.c`. Each part of the code is designed to ensure smooth communication with the server and provide an interactive experience for the user.

  
