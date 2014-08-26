This is a very basic implementation of a software framework for the SHIP experiment which is based on FairRoot. To use this software you need:

1. In case you use SLC6 where the GitHub cert is missing, first do (only once):

    ```bash
    mkdir ~/certs
    curl http://curl.haxx.se/ca/cacert.pem -o ~/certs/cacert.pem
    git config --global http.sslcainfo ~/certs/cacert.pem
    ```

2. Set several required shell variables, needed during the installation and running of the
   different software packages. Put these in your shell's rc file (~/.bashrc or ~/.cshrc).
   For bash:

    ```bash
    export SHIPSOFT=~/ShipSoft
    export SIMPATH=$SHIPSOFT/FairSoftInst
    export FAIRROOTPATH=$SHIPSOFT/FairRootInst
    export FAIRSHIP=$SHIPSOFT/FairShip
    export PYTHONPATH=$FAIRSHIP/python:$PYTHONPATH
    ```

    or for the csh:

    ```bash
    setenv SHIPSOFT ~/ShipSoft
    setenv SIMPATH ${SHIPSOFT}/FairSoftInst
    setenv FAIRROOTPATH ${SHIPSOFT}/FairRootInst
    setenv FAIRSHIP ${SHIPSOFT}/FairShip
    setenv PYTHONPATH ${FAIRSHIP}/python:${PYTHONPATH} (assuming PYTHONPATH is already defined) 
    ```

3. Install [FairSoft](https://github.com/FairRootGroup/FairSoft/tree/dev)


    ```bash
    mkdir $SHIPSOFT
    cd $SHIPSOFT
    git clone -b dev https://github.com/ShipSoft/FairSoft.git
    cd FairSoft
    # On SLC6 do: export FC=gfortran 
    ./configure.sh
    # 1) custom installation ...
    # 1) gcc (on Linux) 5) Clang (on OSX)
    # 3) Optimization
    # 1) Yes (install Simulation) 
    # 2) Internet (install G4 files from internet)
    # 1) Yes (install python bindings)
    # path: $SIMPATH
    ```

4. Install [FairRoot](http://fairroot.gsi.de/?q=node/82)

    ```bash
    cd $SHIPSOFT
    git clone -b dev https://github.com/ShipSoft/FairRoot.git
    cd FairRoot
    mkdir build
    cd build
    cmake -DCMAKE_INSTALL_PREFIX=$FAIRROOTPATH ..
    make
    make install
    ```

    To run the tests do:

    ```bash
    cd $SHIPSOFT/FairRoot/build
    make test
    ```

5. Install the [SHIP](https://github.com/ShipSoft/FairShip.git) software:

    ```bash
    cd $SHIPSOFT (or at any other place XXX, assuming environment variable FAIRSHIP set to XXX/FairShip
    git clone https://github.com/ShipSoft/FairShip.git
    mkdir FairShipRun
    cd FairShipRun
    cmake ../FairShip
    make
    . config.sh    [or source config.csh]
    (there is an issue with check_system.csh, does return wrong answer, should be removed from config.csh, otherwise setup stops
    ```

6. Set environment variables for PYTHON:
    
    for the csh:

    ```bash
    setenv PYTHONPATH ${PYTHONPATH}:${FAIRSHIP}/tools/root/bindings/pyroot:${SIMPATH}/lib:${SIMPATH}/lib/root
    setenv LD_LIBRARY_PATH ${LD_LIBRARY_PATH}:${FAIRSHIP}/genfit-build/lib
    ```

7. Now you can for example simulate some events and run the event display:

    ```bash
    python $FAIRSHIP/macro/run_simPythia.py
    python -i $FAIRSHIP/macro/eventDisplay.py
    // Click on "FairEventManager" (in the top-left pane)
    // Click on the "Info" tab (on top of the bottom-left pane)
    // Increase the "Current Event" to >0 to see the events
    Use quit() or Ctrl-D (i.e. EOF) to exit

