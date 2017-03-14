To generate data transformations based on columns of data.

e.g., drivers_details = FOREACH raw_drivers GENERATE $0 AS driverId, $1 AS name;
