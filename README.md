 
# Two-Tier Flask App with MySQL

This is a simple Flask app that interacts with a MySQL database. The app allows users to submit messages, which are then stored in the database and displayed on the frontend.

## Prerequisites

Before you begin, make sure you have the following installed:

- Git (optional, for cloning the repository)
- EC2 insatnce

## Setup

1. Clone this repository (if you haven't already):

   ```bash
   git clone https://github.com/your-username/your-repo-name.git
   ```

2. Navigate to the project directory:

   ```bash
   cd your-repo-name
   ```

3. Create a `.env` file in the project directory to store your MySQL environment variables:

   ```bash
   touch .env
   ```

4. Open the `.env` file and add your MySQL configuration:

   ```
   MYSQL_HOST=mysql
   MYSQL_USER=your_username
   MYSQL_PASSWORD=your_password
   MYSQL_DB=your_database
   ```

## Usage

1. Update System and Install Required Packages:

   ```bash
   sudo apt update && sudo apt upgrade -y
   sudo apt install python3-pip python3-venv mysql-server -y

   ```

2. Access the Flask app in your web browser:
 ```bash
   - http://ec2-instance_ip:5000
   ```

3. Create the `messages` table in your MySQL database:

   - Use a MySQL client or tool (e.g., phpMyAdmin) to execute the following SQL commands:
   
     ```sql
     CREATE TABLE messages (
         id INT AUTO_INCREMENT PRIMARY KEY,
         message TEXT
     );
     ```
4. Run the Flask App:

   ```bash
   python app.py --host=0.0.0.0 --port=5000

   ```
5. Interact with the app:

   - Visit http://ec2-instance-ip to see the frontend. You can submit new messages using the form.


## Notes

- Make sure to replace placeholders (e.g., `your_username`, `your_password`, `your_database`) with your actual MySQL configuration.

- This is a basic setup for demonstration purposes. In a production environment, you should follow best practices for security and performance.

- Be cautious when executing SQL queries directly. Validate and sanitize user inputs to prevent vulnerabilities like SQL injection.

##  Notes License
<pre>This project is open-source and available under the MIT License.</pre>

## 📬 Contact
<pre>For questions, feedback, or contributions, feel free to open an issue or submit a pull request.</pre>

