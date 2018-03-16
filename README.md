# EMAGIN Model Pipeline Technical Case Study Evaluation

EMAGIN has decided to implement a model predictive control system to reduce the energy consumption of a booster pumping system. The control system is powered by Multi-Input Multi-Output (MIMO) Machine Learning (ML) forecasting models that were trained using historical datasets. The datasets are provided in 5-minute resolution and were produced by the facility's Supervisory Control and Data Acquisition (SCADA) system. 

The models were designed to forecast the pump's key process indicators (i.e. flowrate, pressure) for the next hour. Specifically, a single MIMO model receives a feature space (1 x N) and forecasts the process indicator variable for M timesteps ahead. The target space is thus 1 x M. 

For this preliminary investigation, you will focus on two predictive models:

>	***Feed Flowrate Model***: MIMO ML model that predict the flowrates of feed water entering the pumping system based on seasonal parameters and historical flowrate observations (i.e. the flows at time t+1, t+2, …, t+12 are predicted by a single model as a function of previously recorded flows at time t, t-1, t-2, …, t-36). Note in this case M = 12. In other words, the Feed Flowrate MIMO model predicts the FeedFlowrate 12 timesteps ahead (since each timestep is 5-mins, this represents an hour ahead forecast). 

> ***Primary Pressure Model***: MIMO ML model that predicts the pressure produced by the pump based on the feed flowrates. This model captures the behavior of the P-201 boost pump of the RO system. Note that this implies that forecasts of the feed model are used as inputs to the pressure model. (M = 12 as well)

As part of the real-time integration phase, you have been tasked with building a real-time model execution pipeline that will be able to execute the two core models. Specifically, this pipeline will involve developing a python script that will ingest data from a *.csv file from which, it will:

1.	Retrieve the required latest chunk of data from the *.csv file 
2.	Use this data to create a feature space for the feed flow model. Please see Section 6.0 to see what parameters should be included in the feature space.
3.	Execute the feed flow models to forecast feed flowrates for the next hour (12 timesteps) using the provided model file and the single generated feature space (from Step 2). 
4.	Couple the feed flow forecasts (targets from Step 3) with parameters from the dataset produced in Step 1 to develop the feature space for the primary pressure model. Please see Section 6.0 to see what parameters should be included in the feature space.
5.	Execute the primary pressure model to forecast primary pressure for the next hour (12 timesteps) using the provided model and the generated feature space (from Step 4)

