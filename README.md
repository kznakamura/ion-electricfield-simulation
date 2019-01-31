
## VERSION UPDATE LOG
#### ver.02
 * developed by Kiseki Nakamura on early 2018
 * not working b/c geometry broken and written with un-supported literature for old gmsh-version

   
#### ver.02.01
 * developed by S.Obara on Dec. 2018
 * geometry is almost same as ver02 and confirmed to work with gmsh-ver2.16.0 on MacOSX
 * but wire radius is wrong with actual experiment setup 


#### ver.02.02
 * developed by S.Obara on Dec. 2018
 * modified the wire radius with the experiment setup 
 * for change of wire radius size, re-optimized the charcteristics length parameter
      in order to avoid the "Warning : Volume Mesh: worst distortion"
 * not supported for hD=1.0cm type detector, for the characteristic length optimization
   
#### ver.03.01
 * developed by S.Obara on Jan. 2019
 * changed the setup; less spacer on wire; likely to original Ichikawa's geometry
  
  
## PROCESS
#### How To Simulate 
  1. `[./]$ gmsh ionyomi.geo -3 -order 2 >& gmsh.log;`
  2. `[./]$ mkdir elmergrid;`
  3. `[./]$ ElmerGrid 14 2 ionyomi.msh -autoclean -out ./elmergrid >& elmergrid.log;`
  4. `[./]$ emacs ./elmergrid/dielectrics.dat;`
  5. `[./]$ mkdir elmersolver;`
  6. `[./]$ mkdir elmersolver/[workdir: ex) 1atm_1500V];`
  7. `[./]$ cd elmersolver/[workdir];`
  8. `[./elmersolver/workdir]$ emacs ./ionyomi.sif`(you can copy from ./)
  9. `[./elmersolver/workdir]$ ElmerSolver ionyomi.sif `
  10. `[./elmersolver/workdir]$ field.cxx` (<-- compile on FEM_anal or something)


#### Hint for Garfield++ Compiler
For easy compilation with Garfield++ and ROOT,

  * you can edit environmental variables;
    * (for bash case)
    * ```export CPATH=${CPATH}:${GARFIELD_HOME}/Include```
    * ```alias gg++='g++ `root-config --cflags --libs ` -lGeom -lRint -lgfortran -lm -L${GARFIELD_HOME} -lGarfield'```
  * and use gg++ command as (like a usual g++ command)
    * ` $ gg++ field.cxx`
  * However, it did not work with common garfield++ on haruna; I don't know why.
    * I can use on MacOSX with my installed garfiled++.

                           

## GEOMETRY SIDEVIEW (WIRE DIRECTION)
```

        <------ rD*2 ------>
             <- rH*2 ->

  Vtop   -----------------   ^
         |   |       |   |   |        PTFE tube (gas is filled)
         |   |       |   |   | hD     as "PTFE-A"
         |   |       |   |   |
         |   |       |   |   v
 Vmiddle |---|   *   |---|     tE     Half moon electrode, wire tube, 
         |   |       |   |   ^
         |   |       |   |   | hI
         |   |       |   |   v        PTFE tube as "PTFE-B"
 Vbottom -----------------

                wire
   (length = (rH-gap)*2, radius=rW)

```


## GEOMETRY SIDEVIEW 
```

        <------ rD*2 ------>
             <- rH*2 ->

  Vtop   -----------------   ^
         |   |       |   |   |    
         |   |       |   |   | hD
         |   |       |   |   |
         |   |       |   |   v
 Vmiddle |---| ----- |---|     tE 
         |   |       |   |   ^
         |   |       |   |   | hI
         |   |       |   |   v    
 Vbottom -----------------

            -> <- gap btw wire and ptfe inside wall

  "gap" is for geometry definition with gmsh
      which has no boolean geometry definition.

```


## GEOMETRY COMPONENTS

#### Shape Definition
  * Gas;       Cylinder Shape + thin plate for electrode gap
  * PTFE;      Cylinder Shape with a Hole
  * Electrode; Half Moon Shape
  * Wire;      Cylinder Shape

#### Volume Definition
  * Gas;        Vgas - Vwire
  * PTFE-A;     VptfeA (upper PTFE)
  * PTFE-B;     VptfeB (lower PTFE)
  * Electrode0; Velectrode0 
  * Electrode2; Velectrode2 
  * Wire;       Vwire


## PHYSICAL DEFINITION NUMBERS

#### Physical Surface
  1. Top GND
  2. Middle electrode0
  3. Middle electrode2
  4. Wire
  5. Bottom GND
    
#### Physical Volume
  1. Gas
  2. Wire
  3. Electrode0
  4. Electrode2
  5. PTFE-A
  6. PTFE-B

  

## CREDIT

  * For a Positive Ion Detection Study at MiniChamber
     - to check the electric field or something
  * This gmsh geometry is illustrated by S.Obara on Dec. 2018,
     - developed on MacOSX-ver.10.13.6 with gmsh-ver.2.16.0.
     - while gmsh-ver.4.0.7 is too latest for EmerFem (ver.8.3)
     - ElmerFem ver.8.3 is used

