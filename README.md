### Step-by-Step Instructions

1. *Install PostgreSQL 16.3:*
   bash
   sudo apt update
   sudo apt install postgresql-16
   

2. *Install Nginx:*
   bash
   sudo apt install nginx
   

3. *Configure Nginx:*
   - Edit the default Nginx configuration file:
     bash
     sudo nano /etc/nginx/sites-available/default
     
   - Add your server block configuration (replace with your specific settings).

4. *Start PostgreSQL Server:*
   bash
   sudo systemctl start postgresql
   
   - Ensure PostgreSQL starts on boot:
     bash
     sudo systemctl enable postgresql
     

5. *Clone the Repository:*
   bash
   mkdir -p ~/projects
   cd ~/projects
   git clone https://github.com/choudharypravesh/febe-database
   

6. *Install PM2 Globally:*
   bash
   npm install -g pm2
   

7. *Navigate to the Project Directory and Start the Node.js Server:*
   bash
   cd ~/projects/febe-database
   pm2 start server.js
   

8. *Install soul-cli Globally:*
   bash
   npm install -g soul-cli
   

9. *Create a Directory for Soul and Initialize Soul CLI:*
   bash
   mkdir -p ~/projects/soul
   cd ~/projects/soul
   soul-cli init
   

10. *Start Soul CLI Server Using PM2:*
    bash
    pm2 start soul-cli --name soul-cli -- run --port 3001
    

11. *Ensure PM2 Starts on Boot:*
    bash
    pm2 startup
    pm2 save
    

12. *Verify Running PM2 Processes:*
    bash
    pm2 list
    

### Additional Steps to Ensure Everything is Configured Properly

- *Check Nginx Configuration:*
  bash
  sudo nginx -t
  sudo systemctl restart nginx
  

- *Check PostgreSQL Status:*
  bash
  sudo systemctl status postgresql
  

- *Ensure Environment Variables Are Set (if any):*
  If your server.js or soul-cli require environment variables, make sure to set them up. You can create a .env file in your project directory or export them in your shell.

  bash
  export DB_HOST=localhost
  export DB_PORT=5432
  # ...other variables
  

- *Install Project Dependencies:*
  If your febe-database project or soul-cli project has dependencies, make sure to install them:

  bash
  cd ~/projects/febe-database
  npm install

  cd ~/projects/soul
  npm install
  

### Summary

1. *Install necessary software (PostgreSQL, Nginx, Node.js, PM2, soul-cli).*
2. *Configure and start PostgreSQL and Nginx.*
3. *Clone the febe-database repository and start the Node.js server using PM2.*
4. *Initialize and start the soul-cli server using PM2.*
5. *Ensure services start on boot with PM2.*

By following these steps, you ensure that your services will continue running even after you disconnect from the Linux server.
