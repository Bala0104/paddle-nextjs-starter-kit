# Fees App Project: 01K680WMS7JPXX7NRA1HYFRFG0

## Frontend Components
import React from 'react';
import './TourSteps.css';

const TourStep = ({ step, onNext, onPrev }) => {
  return (
    <div className='tour-step'>
      <h3>{step.title}</h3>
      <p>{step.description}</p>
      <button onClick={onPrev} disabled={step.index === 0}>Previous</button>
      <button onClick={onNext} disabled={step.index === step.total - 1}>Next</button>
    </div>
  );
};

export default TourStep;

// TourSteps.css
.tour-step {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 8px;
  background-color: #f9f9f9;
}
.tour-step button {
  margin: 5px;
  padding: 10px 15px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
.tour-step button:disabled {
  background-color: #e0e0e0;
  cursor: not-allowed;
}


## Backend API
const express = require('express');
const router = express.Router();
const { authenticateJWT } = require('./middleware/auth');
const ToursController = require('./controllers/toursController');

// GET: Retrieve all tours
router.get('/tours', authenticateJWT, ToursController.getAllTours);

// POST: Create a new tour
router.post('/tours', authenticateJWT, ToursController.createTour);

// GET: Retrieve a specific tour
router.get('/tours/:id', authenticateJWT, ToursController.getTourById);

// PUT: Update existing tour
router.put('/tours/:id', authenticateJWT, ToursController.updateTour);

// DELETE: Remove a tour
router.delete('/tours/:id', authenticateJWT, ToursController.deleteTour);

module.exports = router;


## Database Schema
-- Schema for Guided Tours Database
CREATE TABLE Users (
    user_id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50) NOT NULL UNIQUE,
    email VARCHAR(100) NOT NULL UNIQUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE Tours (
    tour_id INT PRIMARY KEY AUTO_INCREMENT,
    tour_name VARCHAR(100) NOT NULL,
    created_by INT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (created_by) REFERENCES Users(user_id)
);

CREATE TABLE Steps (
    step_id INT PRIMARY KEY AUTO_INCREMENT,
    tour_id INT,
    step_order INT NOT NULL,
    content TEXT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (tour_id) REFERENCES Tours(tour_id)
);

-- Stored Procedure for Tour Management
DELIMITER $$
CREATE PROCEDURE AddTour(IN p_username VARCHAR(50), IN p_tour_name VARCHAR(100))
BEGIN
    DECLARE v_user_id INT;
    SELECT user_id INTO v_user_id FROM Users WHERE username = p_username;
    INSERT INTO Tours (tour_name, created_by) VALUES (p_tour_name, v_user_id);
END $$
DELIMITER ;

## Technical Specifications
The project aims to create guided tours in Grist, allowing users to craft interactive walk-throughs and tutorials. This involves integrating Document Tours and Tutorials functionalities to enhance user onboarding experience. The primary technologies involved include Grist API, HTML, CSS, and JavaScript for the front-end components and an SQL database for storing user interactions and tour data.