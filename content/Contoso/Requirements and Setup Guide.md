# Requirements and Setup Guide

This document outlines the requirements and step-by-step instructions for setting up the Contoso Analytics database environment.

---

## Prerequisites

### 1. PostgreSQL Database Installation

Install PostgreSQL database following the video tutorial: [PostgreSQL Installation Guide](https://www.youtube.com/watch?v=4qH-7w5LZsA)

> [!warning] Important Follow the video instructions **up to minute 3:00 only**.

Key steps from the video:

- Download PostgreSQL installer
- Run the installation wizard
- Set up a password for the postgres superuser
- Configure the port (default: 5432)
- Complete the basic installation

---

### 2. DBeaver Community Installation

Download and install DBeaver Community edition from the official website:

🔗 [DBeaver Community | Free Open-Source Database Management Tool](https://dbeaver.io/)

DBeaver is a universal database management tool that will be used to interact with your PostgreSQL database.

---

### 3. Clone the Contoso Analytics Repository

Clone or download the Contoso Analytics repository from GitHub:

**Repository**: [muhssamy/Contoso-Analytics](https://github.com/muhssamy/Contoso-Analytics)

> [!tip] Clone Command
> 
> ```bash
> git clone https://github.com/muhssamy/Contoso-Analytics.git
> ```
> 
> Or download as ZIP from the repository page.

---

## Setup Instructions from Repository

Follow the complete setup instructions provided in the README file within the Contoso Analytics repository.

> [!info] Repository Contents
> 
> - 🗄️ PostgreSQL sample database with 10M+ records
> - Automated data loading scripts
> - Comprehensive setup guide for data analysis, SQL practice, and BI reporting

Refer to the repository's README.md for detailed instructions on:

- Database schema creation
- Data loading procedures
- Connection configuration
- Sample queries and usage examples

---

## Summary

> [!check] Setup Checklist
> 
> - [ ] Install PostgreSQL (first 3 minutes of video)
> - [ ] Install DBeaver Community
> - [ ] Download Contoso Analytics repository
> - [ ] Follow repository README for database setup and data loading

---

**Note**: Make sure to complete each step in order before proceeding to the next one.