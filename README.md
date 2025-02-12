scheduler.js
const appointments = [];
const fs = require('fs');

// Add Appointment (with Validation)
const addAppointment = (clientName, appointmentTime, serviceType) => {
    try {
        if (!clientName || isNaN(new Date(appointmentTime))) {
            throw new Error("Invalid client name or appointment time.");
        }
        const appointment = { clientName, appointmentTime: new Date(appointmentTime), serviceType };
        appointments.push(appointment);
        console.log(`Appointment added: ${clientName} - ${serviceType} at ${appointmentTime}`);
    } catch (error) {
        console.error("Error adding appointment:", error.message);
    }
};

// Get Upcoming Appointments (Next 1 Hour)
const getUpcomingAppointments = () => {
    const now = new Date();
    const oneHourLater = new Date(now.getTime() + 60 * 60 * 1000);
    return appointments.filter(app => app.appointmentTime > now && app.appointmentTime <= oneHourLater);
};

// Send Appointment Reminder
const sendReminder = () => {
    appointments.forEach(app => {
        const timeDiff = new Date(app.appointmentTime) - new Date();
        if (timeDiff > 0) {
            setTimeout(() => {
                console.log(`Reminder: Your appointment for ${app.serviceType} with ${app.clientName} is scheduled at ${app.appointmentTime}`);
            }, timeDiff);
        }
    });
};

// Log Appointments to File
const logAppointments = () => {
    const summary = appointments.map(app => `Client: ${app.clientName}, Service: ${app.serviceType}, Time: ${app.appointmentTime}`).join('\n');
    fs.writeFileSync('summary.txt', summary);
    console.log("Appointments logged in summary.txt");
};

// Example Usage
addAppointment("John Doe", "2025-02-12T10:30:00", "Consultation");
addAppointment("Alice Smith", "2025-02-12T11:00:00", "Follow-up");

console.log("Upcoming Appointments:", getUpcomingAppointments());
sendReminder();
logAppointments();

// Exporting Functions for Testing
module.exports = { addAppointment, getUpcomingAppointments, sendReminder, logAppointments };

expenseTracker.js
const expenses = [];
const fs = require('fs');

const addExpense = (description, amount, date) => {
    try {
        if (!description || amount <= 0 || isNaN(new Date(date))) {
            throw new Error("Invalid expense details.");
        }
        const expense = { description, amount, date: new Date(date) };
        expenses.push(expense);
        console.log(`Expense added: ${description} - $${amount} on ${date}`);
    } catch (error) {
        console.error("Error adding expense:", error.message);
    }
};

const getTotalExpenses = () => {
    return expenses.reduce((total, expense) => total + expense.amount, 0);
};

const filterExpensesByDateRange = (startDate, endDate) => {
    const start = new Date(startDate);
    const end = new Date(endDate);
    return expenses.filter(exp => exp.date >= start && exp.date <= end);
};

const fetchExpenseReport = async () => {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            if (expenses.length > 0) {
                resolve(`Total Expenses: $${getTotalExpenses()}`);
            } else {
                reject("No expenses recorded.");
            }
        }, 2000);
    });
};

addExpense("Lunch", 15, "2025-02-12T12:30:00");
addExpense("Books", 30, "2025-02-11T10:00:00");

console.log("Total Expenses:", getTotalExpenses());
console.log("Filtered Expenses:", filterExpensesByDateRange("2025-02-10", "2025-02-12"));

fetchExpenseReport()
    .then(report => console.log(report))
    .catch(error => console.error(error));

module.exports = { addExpense, getTotalExpenses, filterExpensesByDateRange, fetchExpenseReport };

studyPlanner.js
const studySessions = [];
const fs = require('fs');

const addSession = (topic, sessionTime, duration) => {
    try {
        if (!topic || duration <= 0 || isNaN(new Date(sessionTime))) {
            throw new Error("Invalid session details.");
        }
        const session = { topic, sessionTime: new Date(sessionTime), duration };
        studySessions.push(session);
        console.log(`Session added: ${topic} for ${duration} minutes at ${sessionTime}`);
    } catch (error) {
        console.error("Error adding session:", error.message);
    }
};

const listTodaysSessions = () => {
    const today = new Date().toISOString().split('T')[0];
    return studySessions.filter(session => session.sessionTime.toISOString().split('T')[0] === today);
};

const sessionCountdown = (topic, sessionTime) => {
    const delay = new Date(sessionTime) - new Date();
    if (delay > 0) {
        setTimeout(() => console.log(`Session on ${topic} starts now!`), delay);
    }
};

const fetchStudyMaterials = async (topic) => {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            if (topic) {
                resolve(`Study materials for ${topic} are ready.`);
            } else {
                reject("No topic provided.");
            }
        }, 2000);
    });
};

addSession("Math", "2025-02-12T14:00:00", 60);
addSession("Physics", "2025-02-12T16:00:00", 45);

console.log("Today's Sessions:", listTodaysSessions());
sessionCountdown("Math", "2025-02-12T14:00:00");

fetchStudyMaterials("Math")
    .then(materials => console.log(materials))
    .catch(error => console.error(error));

module.exports = { addSession, listTodaysSessions, sessionCountdown, fetchStudyMaterials };
