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
 * clk_voltage="3.3V"
 * clock="1GHz"
 * cpu_cluster="1.2V"
 * mem-type, default="DDR3_1600_8x8"
 * memoryMode() == "timing"
 * cpu-freq, default="4GHz"
 * num-cores, default=1
 * mem-size,  default="2GB"
### 2)
    a)
      Στα αρχεία config.ini και config.json βρίσκουμε τα παρακάτω που επιβεβαιώνουν 
      κάποια από τα παραπάνω.
      mem_mode=timing
      sys_clk :
      voltage=3.3V
      clock=1000 ticks δηλαδή 1GHz 
   
      Cpu_cluster :
      voltage=1.2V
      clock=250 ticks δηλαδή 4GHz
      
      memory size
      range=0:2147483648:0:128 δηλαδή 2Gb

    b)
       Από το αρχείο stats.txt προκύπτει ότι :
       system.cpu_cluster.cpus.committedInsts  5028  # Number of instructions committed
       sim_insts 5028 # Number of instructions simulated
       Κάνοντας και την πράξη numCycles/CPI = 97284/19.348449 = 5027,99 το ίδιο σχεδόν με 
       παραπάνω.

    c) 
       system.cpu_cluster.l2.overall_accesses::total 479 
       # number of overall (read+write) accesses
       Αν δεν μας έδινε τα συνολικά θα έπρεπε να προσθέσουμε όλα τα ξεχωριστά accesses 
### 3) 
 Όπως αναφέρεται και παραπάνω στο _[συνημμένο](https://github.com/arm-university/arm-gem5-rsk/blob/master/gem5_rsk.pdf)_ περιέχοντε πληροφορίες για τα μοντέλα in-order CPU του gem5 που θα αναλύσω περιλιπτικά παρακάτω : 
 * AtomicSimpleCPU 
     * O AtomicSimpleCPU χρησιμοποιεί ατομική προσπέλαση στην μνήμη, η οποία είναι και η πιο γρήγορη από τα είδη των προσπελάσεων στην μνήμη και πραγματοποεί ένα transaction σε μια μόνο κλήση συνάρτησης. Ο AtomicSimpleCPU πραγματοποιεί όλες τις απαραίτητες ενέργειες για μια εντολή σε κάθε CPU tick , και μπορεί να κάνει έναν υπολογισμό από όλα τα cache accesses που έγιναν , υπολογίζοντας το latency από τις ατομικές προσπελάσεις. Φυσιολογικά , ο AtomicSimpleCPU έχει την πιο γρήγορη προσωμοίωση ,και χρησιμοποιείται για fast-forwarding ώστε να βρεθεί στο Region of Interest (ROI) στον gem5.
* TimingSimpleCPU 
     * O TimingSimpleCPU χρησιμοποιεί Timing προσπέλαση στην μνήμη. Αυτό σημαίνει οτί περιμένει η προσπέλαση στην μνήμη να επιστρέψει πριν προχωρήσει , και για αυτό προσδίδει κάποιο επίπεδο χρονισμού. O TimingSimpleCPU είναι επίσης και fast-to-run μοντέλο, αφού απλοποιεί κάποιες πτυχές συμπεριλαμβάνοντας το pipelining, που σημαίνει ότι μόνο μια εντολή επεξεργάζεται κάθε στιγμή. Κάθε αριθμιτική εντολή εκτελείται σε ένα κύκλο , ενώ η προσπέλαση στη μνήμη απαιτεί πολλαπλούς κύκλους.
* MinorCPU 
     * O MinorCPU ,που χρησιμοποιούμε και εμείς, είναι ένα εύελικτο in-order μοντέλο επεξεργαστή που αναπτύχθηκε από την ARM ISA. Ο MinorCPU έχει ένα προσαρμσμένο 4-σταδίων execution pipeline, ενώ έχει διαμορφώσιμες δομές δεδομένων και συμπεριφορά εκτέλεσης. Για αυτό μπορεί να διαμορφωθεί σε μικρο-αρχιτεκτονικό επίπεδο , να μοντελοποιήσει έναν συγκεκριμένο επεξεργαστή.
* High-Perfomance In-Order CPU.
     * O HPI CPU timing model είναι ρυθμιζόμενος ώστε να είναι αντιπροσωπευτικός ενός in-order ARMv8-A.
#### a) 
     MinorCPU 
     sim_seconds : 0.000032   # Number of seconds simulated
     system.cpu.numCycles : 64058  # number of cpu cycles simulated
     system.cpu.committedInsts : 8313   # Number of instructions committed
     
     TimingSimpleCPU 
     sim_seconds : 0.000037    # Number of seconds simulated
     system.cpu.numCycles : 73088   # number of cpu cycles simulated
     system.cpu.committedInsts : 8258  # Number of instructions committed
#### b)
      Ο χρόνος του MinorCPU είναι μικρότερος από τον χρόνο του TimingSimpleCPU , όπως και αναμέναμε, αφού ο TimingSimpleCPU περιμένει να τελειώσει η προσπέλαση στην μνήμη. Δε διαθέτει pipeling αντιθέτως με το MinorCPU , που διαθέτει 4-σταδιών execution pipeline. Παρόλο που ο TimingSimpleCPU έκανε περισσότερους κύκλους ,εκτέλεσε λιγότερες εντολές από τον MinorCPU καθώς η προσπέλαση στην μνήνη απαιτεί πολλαπλούς κύκλους όπως και αναφέρθηκε.

#### c) 
Αλλάζουμε πρώτα την συχνότητα από *1GHz* σε *400ΜHz* :

* MinorCPU 
  * sim_seconds : 0.000041  # Number of seconds simulated
* TimingSimpleCPU
  * sim_seconds : 0.000045  # Number of seconds simulated

Παρατηρούμε ότι οι χρόνοι είναι μεγαλύτεροι από πριν, καθώς μειώσαμε την συχνότητα.
Αλλάζουμε την τεχνολογία μνήμης από την default *DDR3_1600_8x8* σε *DDR4_2400_8x8* :

* MinorCPU 
  * sim_seconds : 0.000030  # Number of seconds simulated
* TiminingSimpleCPU
  * sim_seconds : 0.000036  # Number of seconds simulated

Παρατηρούμε ότι οι χρόνοι είναι μικρότεροι από πριν, όπως και αναμέναμε αφού βάλαμε καλύτερης τεχνολογίας μνήμη.







