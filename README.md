## 1ο ΜΕΡΟΣ
Από το αρχείο starter_se.py μπορούμε να βγάλουμε τα παρακάτω συμπεράσματα:
### 1)
 * cpu_types = {
   * "atomic" : ( AtomicSimpleCPU, None, None, None, None),
    * "minor" :        
         *  (MinorCPU, devices.L1I, devices.L1D,
               devices.WalkCache,
               devices.L2), 
    * "hpi" : 
        *    ( HPI.HPI,  HPI.HPI_ICache, HPI.HPI_DCache,
              HPI.HPI_WalkCache,
              HPI.HPI_L2) }

*Στην περίπτωση μας χρησιμοποιήσαμε τύπου "minor", άρα και τις L1 L2 caches.
 O minorCPU είναι ενας in-order CPU. Περισσότερες πληροφορίες στην αντίστοιχη ενότητα
 στο _[συνημμένο](https://github.com/arm-university/arm-gem5-rsk/blob/master/gem5_rsk.pdf)_*
 * voltage="3.3V"
 * clock="1GHz"
 * cpu_cluster="1.2V"
 * mem-type, default="DDR3_1600_8x8"
 * memoryMode() == "timing"
 * cpu-freq, default="4GHz"
 * num-cores, default=1
 * mem-size,  default="2GB"
