Hi there,
this wiki page introduce you to the use of tools provided by LTE-Sim.


### make\_plr ###
it computes the packet loss ratio starting from the number of sent packet and the number of received packet.

Example: PLR fo the application layer
```
./LTE-Sim SingleCellWithI 7 1 1 0 0 1 0 1 1 3 0.1 128 > trace.tr
grep -c "^TX" trace.tr > tmp
grep -c "^RX" trace.tr >> tmp
./src/Simulation/TOOLS/make_plr  tmp
```


### make\_cell\_spectral\_efficiency ###
it computes the total cell spectral efficiency as the sum of bits correctly received by nodes into a cell for bandwidth and simulation times. It is expressed in bits/Hz/s.

Example: we suppose to run a simulation with bandwidth = 10Mhz and duration = 30 seconds.
```
./LTE-Sim SingleCellWithI 7 1 1 0 0 1 0 1 1 3 0.1 128 > trace.tr
bandwidth=10000000
time=30
grep "^RX" trace.tr  | awk '{print $8}'  > tmp
./src/Simulation/TOOLS/make_cell_spectral_efficiency   tmp   ${time}   ${bandwidth} 
```


### make\_goodput ###
it computes the  goodput of a given flow as the ratio between the sum of bits correctly received and the simulation time.

Example:
```
./LTE-Sim SingleCellWithI 7 1 1 0 0 1 0 1 1 3 0.1 128 > trace.tr
time=30
grep "RX INF_BUF"   trace.tr  | awk '{print $8}'  > tmp
./src/Simulation/TOOLS/make_goodput  tmp ${time} 
```


### make\_fairness\_index ###
it computes the  FI among flows by using the Jain's fairness index.

Example:
```
./LTE-Sim SingleCellWithI 7 1 10 0 0 1 0 1 1 3 0.1 128 > trace.tr
time=30
users=10
for bearer in $(seq 0 1 $(( ${users}- 1 )))
do 
        grep "RX INF_BUF"   trace.tr  | grep "B ${bearer} " |  awk '{print $8}'  > tmp
        ./src/Simulation/TOOLS/make_goodput  tmp ${time}  >>tmp_2
done 
./src/Simulation/TOOLS/make_fairness_index  tmp_2  
```


### make\_cdf ###
it computes the cumulative distribution function of packet delays

Example: scenario with 10 video flows
```
./LTE-Sim SingleCellWithI 7 1 10 0 10 0 0 1 1 3 0.1 128 > trace.tr
grep "RX VIDEO"  trace.tr  |  awk '{print $14}' > tmp
./src/Simulation/TOOLS/make_cdf     tmp     cdf_of_delays.tr  
```

cdf\_of\_delays.tr will have two colums. The first one is the delay and the second one is the CDF value.