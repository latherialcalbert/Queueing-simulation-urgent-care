# Queue Simulation: M/M/1 with Reneging
**Latherial Calbert & John McMann**

This project simulates an M/M/1 queueing system in MATLAB using an event-driven architecture. 

This version adds support for customer reneging, where waiting customers leave the system after a random patience time.

## What Changed: Reneging Feature

Two new files were added to support reneging:

- **`Renege.m`** -- An event class that is called when a waiting customer's patience expires. It calls `handle_renege` on the queue, which removes the customer from the waiting line and records them as reneged.

- **`ServiceQueueRenege.m`** -- A modified version of `ServiceQueue` that adds a `RenegeRate` property, a `RenegeDist` exponential distribution, and a `Reneged` cell array to track customers who left early. The `handle_arrival` method now schedules a `Renege` event for each patient who arrives to a busy server. The log table includes a fifth column `NumReneged`. A `handle_renege` method searches the waiting list for the target customer and removes them if they have not yet entered service.

## Architecture

The simulation is event-driven. The main class `ServiceQueue` maintains a priority queue of events ordered by time. At any moment there is one scheduled `Arrival`(the next patient), one `Departure` per busy server, and one `RecordToLog` (the
next log entry).`ServiceQueueRenege` adds a `Renege` event for each waiting patient.

## Live Scripts

**`Run_ServiceQueue_baseline.m`**

Models Chucktown Urgent Care as a standard M/M/1 queue with $\lambda = 2$ arrivals per hour and $\mu = 3$ patients per hour. Simulates 20 independent 8-hour shifts and computes theoretical and simulated values for $P_0$ through $P_5$, $L$, $L_q$,
$W$, and $W_q$. Generates histograms of customers in system, customers waiting,customers in service, time in system, time in queue, time in service, and customers served per shift.

**`Run_ServiceQueueRenege.m`**

Extends the baseline to include reneging with a mean patience time of 15 minutes ($\theta = 4$ per hour). Computes theoretical $P_n$ using the hypergeometric formula, $\pi_s$ (fraction of patients served), $L$, $L_q$, $W$, and $W_q$.
Simulates 20 independent 8-hour shifts and compares all five measures against theory. Generates histograms of customers in system, time in system, time in queue, time in service, customers served per shift, and customers reneged per shift.

---
*Note: "queueing" rather than "queuing" follows the convention of the course textbook.*