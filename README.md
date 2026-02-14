# KTSS_L4SOD_Notes
Notes for L4 SOD Both 3 Courses 


//codes

//creating CRUD operations for users table
const express = require('express');
const app = express();
const connection = require('./db');
app.use(express.json());
//GET all users
app.get('/users', (req, res) => {
    connection.query('SELECT * FROM users', (err, results) => {
        if (err) {
            console.error('Error fetching users:', err);
            res.status(500).json({ error: 'Failed to fetch users' });
        }else {
            res.json(results);
        }
    });
});

//GET Users by ID
app.get('/users/:id', (req, res) => {
    const userId = req.params.id;
    const query = `SELECT * FROM users WHERE id = ?`;
    connection.query(query, [userId], (err, results) => {
        if (err) {
            console.error('Error fetching user:', err);
            res.status(500).json({ error: 'Failed to fetch user' });
        } else if (results.length === 0) {
            res.status(404).json({ error: 'User not found' });
        }
        else {            
            res.json(results[0]);
        }
    });
});

// Create a new user
app.post('/users', (req, res) => {
    const { Name, sex, phone, district, school, trade, module } = req.body;
    const query = `INSERT INTO users (Name, sex, phone, district, 
    school, trade, module) VALUES (?, ?, ?, ?, ?, ?, ?)`;
    connection.query(query, [Name, sex, phone, district, school, trade, module], (err, results) => {
        if (err) {
            console.error('Error creating user:', err);
            res.status(500).json({ error: 'Failed to create user' });
        } else {
            res.status(201).json({ message: 'User created successfully', userId: results.insertId });
        }
    });
});

// Update a user
app.put('/users/:id', (req, res) => {
    const userId = req.params.id;
    const { Name, sex, phone, district, school, trade, module } = req.body;
    const query = `UPDATE users SET Name = ?, sex = ?, phone = ?, 
    district = ?, school = ?, trade = ?, module = ? WHERE id = ?`;

    connection.query(query, [Name, sex, phone, district, school, trade, module, userId], (err, results) => {
        if (err) {
            console.error('Error updating user:', err);
            res.status(500).json({ error: 'Failed to update user' });
        } else {
            res.json({ message: 'User updated successfully' });
        }
    });
});

// Delete a user
app.delete('/users/:id', (req, res) => {
    const userId = req.params.id;
    const query = `DELETE FROM users WHERE id = ?`;
    connection.query(query, [userId], (err, results) => {
        if (err) {
            console.error('Error deleting user:', err);
            res.status(500).json({ error: 'Failed to delete user' });
        }
        else {
            res.json({ message: 'User deleted successfully' });
        }
    });
});

app.listen(3000, () => {
    console.log('Server is running on port http://localhost:3000');
});
