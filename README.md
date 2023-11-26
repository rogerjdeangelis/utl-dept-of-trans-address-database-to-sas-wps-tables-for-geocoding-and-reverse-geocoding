# utl-dept-of-trans-address-database-to-sas-wps-tables-for-geocoding-and-reverse-geocoding
Dept_of_Trans(DOT) address database to sas/wps table for geocoding and reverse geocoding 
    %let pgm=utl-dept-of-trans-address-database-to-sas-wps-tables-for-geocoding-and-reverse-geocoding;

    Dept_of_Trans(DOT) address database to sas/wps table for geocoding and reverse geocoding

    Note you need to clean your addresses and create a match code using https://tinyurl.com/yck8yu68

                                                                                                                                                                            
    Related repositories                                                                                                                                                 
                                                                                                                                                                         
    https://tinyurl.com/yck8yu68                                                                                                                                         
    https://tinyurl.com/4a94kyh3                                                                                                                                         
    https://github.com/rogerjdeangelis/utl_geocode_and_reverse_geocode_netherland_addresses_and_latitudes_longitudes                                                     
    https://github.com/rogerjdeangelis/utl_geocode_reverse_geocode                                                                                                       
    https://github.com/rogerjdeangelis/utl_US_address-standardization      
    
    Downloads

     1.  Final augmnented data (should be slighly better than previous repository  https://tinyurl.com/yck8yu68 )
         Full open address database and dept of trans addresses (variables:  adr, lat lon 141+ million 1,061,533,499 bytes )
         Onedrive download csv
         https://1drv.ms/u/s!AovFHZtMPA-7giK01NZ3-dl6KtfJ?e=dpcwiX
         You will need the stadarddizing code in https://tinyurl.com/yck8yu68 to fix your input messy addresses.

         * convert to wps/sas table;
         data wps_table;
           length zip4 $4;
           infile 'g:/dotOpn/adr_010opnnadavg_adr_lat_lon.csv' firstobs=2 delimiter=',' DSD lrecl=32767;
           informat adr $96. avglat avglon 11.6;
           format avglat avglon 11.6;
           input adr avglat avglon;
           matchcode=compress(adr);
           zip4=scan(adr,-1," ");
           *if _n_=2000000 then stop;
         run;quit;

         ZIP4    ADR                     AVGLAT      AVGLON    MATCHCODE

         5062    0 1ST ST 50625          -92.972    42.7490    01STST50625
         5060    0 3RD ST 50602          -92.801    42.7526    03RDST50602
         5061    0 CHURCH ST 50611       -92.908    42.7750    0CHURCHST50611


     2,  Just the Dept of Trans adr, lat and lon (output from this repo)
         onedrive download csv  941,447,225
         https://1drv.ms/u/s!AovFHZtMPA-7giVU9Svz1aZpbV9F?e=rW5ZZg

         Source

         https://tinyurl.com/327p452e
         https://www.transportation.gov/mission/open/gis/national-address-database/national-address-database-nad-disclaimer

         * convert to wps/sas table
         data wps_table;
           length zip4 $4;
           infile 'g:/dot/adr010nadavg.csv' firstobs=2 delimiter=',' missover DSD lrecl=32767;
           informat adr $96.;
           input adr avglat avglon;
           matchcode=compress(adr);
           zip4=scan(adr,-1," ");
           *if _n_=2000000 then stop;
         run;quit;

         ZIP4    ADR                        AVGLAT     AVGLON     MATCHCODE

         8751    0 E HIGH ST 87519         36.7081    -105.404    0EHIGHST87519
         0704    0 GLN RD 07046            40.8938     -74.433    0GLNRD07046
         8751    0 N CARIBEL TRL 87519     36.7136    -105.411    0NCARIBELTRL87519


     3.  Zipcode centroids lattitude and longitude
         https://1drv.ms/u/s!AovFHZtMPA-7gh6PgVepoPUV2_dI?e=shjVT9

     4.  Dept of Trans raw address point raw data
         https://www.transportation.gov/mission/open/gis/national-address-database/national-address-database-nad-disclaimer
         You need to download and unzip


    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  WORK.DOT_AND_OPENADDRES total  141,432,837 (Oregon has 0 numbered addresses)                                          */
    /*                                                                                                                        */
    /*  Obs    ADR                               LAT        LON      MATCHCODE                                                */
    /*                                                                                                                        */
    /*    1    0 1ST ST 50625                  -92.972    42.7490    01STST50625                                              */
    /*    2    0 3RD ST 50602                  -92.801    42.7526    03RDST50602                                              */
    /*    3    0 CHURCH ST 50611               -92.908    42.7750    0CHURCHST50611                                           */
    /*    4    0 E RIDGE CTS 50636             -92.796    42.8984    0ERIDGECTS50636                                          */
    /*    5    0 MONON TRL 46280               -86.136    39.9272    0MONONTRL46280                                           */
    /*    6    0 N 1ST ST 50636                -92.805    42.8983    0N1STST50636                                             */
    /*    7    0 N FREDERICK AVE 20879         -77.225    39.1634    0NFREDERICKAVE20879                                      */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*
    Below is sas/wps code to convert the 7000+ US json files to one small(900mb) wps/sas table
    with over 70+ million zipcoded addresses.


    RELATED SOURCES
    ================

        https://bcdcspatial.blogspot.com/2012/09/normalize-to-usps-street-abbreviations.html

        https://batch.openaddresses.io/data#map=0.45/ 23.2/46.3

        https://tinyurl.com/4a94kyh3
        https://github.com/rogerjdeangelis/utl-openaddress-database-to-sas-wps-tables-for-geocoding-and-reverse-geocoding

    /*   _       _
      __| | ___ | |_   _ __ __ ___      __
     / _` |/ _ \| __| | `__/ _` \ \ /\ / /
    | (_| | (_) | |_  | | | (_| |\ V  V /
     \__,_|\___/ \__| |_|  \__,_| \_/\_/

    */

    GET DEPT OF TRANS RAW ADRESSES

    ========================

    https://tinyurl.com/327p452e
    https://www.transportation.gov/mission/open/gis/national-address-database/national-address-database-nad-disclaimer

    Download, unzip and run the code below to create the address 70+ million table.
    Full code below

    libname nad "m:/nad";

    data nad.nad_20231101 (compress=binary ); /*---- 160 variables per address ----*/

      label OID="Col1";
      label AddNum_Pre="Col2";
      label Add_Number="Col3";
      ...
      label NAD_Source="Col59";
      label DataSet_ID="Col60";


      informat AddNum_Pre $15.;
      informat AddNum_Suf $15.;
      informat AddNo_Full $100.;
      ...
      informat NAD_Source $75.;
      informat DataSet_ID $254.;

      infile "d:/adr/NAD20231101.txt" lrecl=32756 recfm=v MISSOVER DSD firstobs=2;

      INPUT
      OID
      AddNum_Pre
      Add_Number
      ...
      NAD_Source
      DataSet_ID
      ;

      if mod(_n_,1000000)=0 then put _n_ comma18.;

      run;quit;

    USAGE
    =====

    Ones you compile all the macros and just run the driver

    This can take a very long time, several hours if run sequentially.
    I suggest you use SPSE and a parralel index and split the states in 30
    separate tasks. NVMe drives are best,

    %stsnad(AK);
    %stsnad(AL);
    %stsnad(AR);
    ...
    %stsnad(WV);
    %stsnad(WY);
    %stsnad(TX);


    */
    /*         _     _       _                     _
      __ _  __| | __| |  ___(_)_ __   ___ ___   __| | ___  ___
     / _` |/ _` |/ _` | |_  / | `_ \ / __/ _ \ / _` |/ _ \/ __|
    | (_| | (_| | (_| |  / /| | |_) | (_| (_) | (_| |  __/\__ \
     \__,_|\__,_|\__,_| /___|_| .__/ \___\___/ \__,_|\___||___/
                              |_|
     _                   _
    (_)_ __  _ __  _   _| |_ ___
    | | `_ \| `_ \| | | | __/ __|
    | | | | | |_) | |_| | |_\__ \
    |_|_| |_| .__/ \__,_|\__|___/
            |_|

    */
    https://batch.openaddresses.io/data#map=0.45/ 23.2/46.3
    https://bcdcspatial.blogspot.com/2012/09/normalize-to-usps-street-abbreviations.html

         _                     _        _       _     _
     ___(_)_ __   ___ ___   __| | ___  | | __ _| |_  | | ___  _ __
    |_  / | `_ \ / __/ _ \ / _` |/ _ \ | |/ _` | __| | |/ _ \| `_ \
     / /| | |_) | (_| (_) | (_| |  __/ | | (_| | |_  | | (_) | | | |
    /___|_| .__/ \___\___/ \__,_|\___| |_|\__,_|\__| |_|\___/|_| |_|
          |_|
    */

    /*----                                                                   ----*/
    /*---- https://1drv.ms/u/s!AovFHZtMPA-7gh6PgVepoPUV2_dI?e=shjVT9         ----*/
    /*---- wps/sas zipcode dataset                                           ----*/
    /*----                                                                   ----*/

    data adr_010UsPlaceZip;
       informat
           LONGITUDE 11.6
           LATITUDE 11.6
           POSTALCODE $5.
           ADMIN_CODE1 $2.
       ;
       /*---- https://1drv.ms/u/s!AovFHZtMPA-7gh6PgVepoPUV2_dI?e=shjVT9      ----*/

       infile 'm:/adr/csv/adr_010UsPlaceZip.csv' firstobs=2 delimiter=',' DSD lrecl=32767;

       input
          LONGITUDE
          LATITUDE
          POSTALCODE
          ADMIN_CODE1
       ;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  last 41 obs from ADR_010USPLACEZIP total obs=41,140                                                                   */
    /*                                                  ADMIN_                                                                */
    /*    Obs    LONGITUDE    LATITUDE    POSTALCODE    CODE1                                                                 */
    /*                                                                                                                        */
    /*  41100     -167.436     65.6638      99783         AK                                                                  */
    /*  41101     -163.261     64.8683      99784         AK                                                                  */
    /*  41102     -166.825     65.4594      99785         AK                                                                  */
    /*  41103     -157.868     67.0846      99786         AK                                                                  */
    /*  41104     -145.274     66.5647      99788         AK                                                                  */
    /*  41105     -151.126     70.2358      99789         AK                                                                  */
    /*  41106     -147.724     64.8430      99790         AK                                                                  */
    /*  41107     -157.428     70.4722      99791         AK                                                                  */
    /*  41108     -134.527     58.3565      99801         AK                                                                  */
    /*  41109     -134.427     58.3042      99802         AK                                                                  */
    /*  41110     -134.593     58.3740      99803         AK                                                                  */
    /*  41111     -134.158     58.4750      99811         AK                                                                  */
    /*                                                                                                                        */
    /**************************************************************************************************************************/
    /*                                                    _   _  ___
     _ __ ___   __ _  ___ _ __ ___  ___    __ _ _ __   __| | (_)/ (_)_ __   ___
    | `_ ` _ \ / _` |/ __| `__/ _ \/ __|  / _` | `_ \ / _` |   / /| | `_ \ / __|
    | | | | | | (_| | (__| | | (_) \__ \ | (_| | | | | (_| |  / /_| | | | | (__
    |_| |_| |_|\__,_|\___|_|  \___/|___/  \__,_|_| |_|\__,_| /_/(_)_|_| |_|\___|
                                                     _ _
     _ __ ___ _ __ ___   _____   _____   _   _ _ __ (_) |_ ___
    | `__/ _ \ `_ ` _ \ / _ \ \ / / _ \ | | | | `_ \| | __/ __|
    | | |  __/ | | | | | (_) \ V /  __/ | |_| | | | | | |_\__ \
    |_|  \___|_| |_| |_|\___/ \_/ \___|  \__,_|_| |_|_|\__|___/

    */

    %macro unit(unit);

      if index(adr," &unit ") then do;
         *put adr=;
         idx=index(adr," &unit " );
         nxt=scan(substr(adr,idx),2);
         *put nxt=;
         wrdlen=length(catx(" ","&unit" ,strip(nxt)));
        *put wrdlen=;
         substr(adr,idx+1,wrdlen)="";
         *put adr=;
         adr=compbl(adr);
         *put adr= // ;
         flg="UN";
      end;

    %mend unit;


    /*   _                  _               _              __  __ _
     ___| |_ __ _ _ __   __| | __ _ _ __ __| |  ___ _   _ / _|/ _(_)_  __
    / __| __/ _` | `_ \ / _` |/ _` | `__/ _` | / __| | | | |_| |_| \ \/ /
    \__ \ || (_| | | | | (_| | (_| | | | (_| | \__ \ |_| |  _|  _| |>  <
    |___/\__\__,_|_| |_|\__,_|\__,_|_|  \__,_| |___/\__,_|_| |_| |_/_/\_\

    */

    filename ft15f001 "%sysfunc(pathname(work))/adr_010sfx.sas";
    parmcards4;
    when (strip(sfx)='BOULEVARD') substr(adr,length(adr)-len) =' BLVD';
    when (strip(sfx)='HARBOR') substr(adr,length(adr)-len) =' HBR';
    when (strip(sfx)='TRPK') substr(adr,length(adr)-len) =' TPKE';
    when (strip(sfx)='FORGES') substr(adr,length(adr)-len) =' FRGS';
    when (strip(sfx)='BYPAS') substr(adr,length(adr)-len) =' BYP';
    when (strip(sfx)='VIADUCT') substr(adr,length(adr)-len) =' VIA';
    when (strip(sfx)='MNT') substr(adr,length(adr)-len) =' MT';
    when (strip(sfx)='LNDNG') substr(adr,length(adr)-len) =' LNDG';
    when (strip(sfx)='VILL') substr(adr,length(adr)-len) =' VLG';
    when (strip(sfx)='MILL') substr(adr,length(adr)-len) =' ML';
    when (strip(sfx)='CENTERS') substr(adr,length(adr)-len) =' CTRS';
    when (strip(sfx)='CNTER') substr(adr,length(adr)-len) =' CTR';
    when (strip(sfx)='HRBOR') substr(adr,length(adr)-len) =' HBR';
    when (strip(sfx)='TR') substr(adr,length(adr)-len) =' TRL';
    when (strip(sfx)='PASSAGE') substr(adr,length(adr)-len) =' PSGE';
    when (strip(sfx)='WALKS') substr(adr,length(adr)-len) =' WALK';
    when (strip(sfx)='CREST') substr(adr,length(adr)-len) =' CRST';
    when (strip(sfx)='MEADOWS') substr(adr,length(adr)-len) =' MDWS';
    when (strip(sfx)='FREEWY') substr(adr,length(adr)-len) =' FWY';
    when (strip(sfx)='GARDEN') substr(adr,length(adr)-len) =' GDN';
    when (strip(sfx)='BLUFFS') substr(adr,length(adr)-len) =' BLFS';
    when (strip(sfx)='TRK') substr(adr,length(adr)-len) =' TRAK';
    when (strip(sfx)='SQUARES') substr(adr,length(adr)-len) =' SQS';
    when (strip(sfx)='FRRY') substr(adr,length(adr)-len) =' FRY';
    when (strip(sfx)='DIV') substr(adr,length(adr)-len) =' DV';
    when (strip(sfx)='STRAVEN') substr(adr,length(adr)-len) =' STRA';
    when (strip(sfx)='CMP') substr(adr,length(adr)-len) =' CP';
    when (strip(sfx)='GRDNS') substr(adr,length(adr)-len) =' GDNS';
    when (strip(sfx)='VILLG') substr(adr,length(adr)-len) =' VLG';
    when (strip(sfx)='MEADOW') substr(adr,length(adr)-len) =' MDW';
    when (strip(sfx)='TRAILS') substr(adr,length(adr)-len) =' TRL';
    when (strip(sfx)='STREETS') substr(adr,length(adr)-len) =' STS';
    when (strip(sfx)='PRAIRIE') substr(adr,length(adr)-len) =' PR';
    when (strip(sfx)='CRESCENT') substr(adr,length(adr)-len) =' CRES';
    when (strip(sfx)='PORT') substr(adr,length(adr)-len) =' PRT';
    when (strip(sfx)='BLUF') substr(adr,length(adr)-len) =' BLF';
    when (strip(sfx)='AVNUE') substr(adr,length(adr)-len) =' AVE';
    when (strip(sfx)='LIGHTS') substr(adr,length(adr)-len) =' LGTS';
    when (strip(sfx)='HARBORS') substr(adr,length(adr)-len) =' HBRS';
    when (strip(sfx)='LODG') substr(adr,length(adr)-len) =' LDG';
    when (strip(sfx)='TRACKS') substr(adr,length(adr)-len) =' TRAK';
    when (strip(sfx)='PKWAY') substr(adr,length(adr)-len) =' PKWY';
    when (strip(sfx)='BOT') substr(adr,length(adr)-len) =' BTM';
    when (strip(sfx)='DRV') substr(adr,length(adr)-len) =' DR';
    when (strip(sfx)='DIVIDE') substr(adr,length(adr)-len) =' DV';
    when (strip(sfx)='FORDS') substr(adr,length(adr)-len) =' FRDS';
    when (strip(sfx)='AVENU') substr(adr,length(adr)-len) =' AVE';
    when (strip(sfx)='RIVR') substr(adr,length(adr)-len) =' RIV';
    when (strip(sfx)='GATEWAY') substr(adr,length(adr)-len) =' GTWY';
    when (strip(sfx)='STREAM') substr(adr,length(adr)-len) =' STRM';
    when (strip(sfx)='BAYOO') substr(adr,length(adr)-len) =' BYU';
    when (strip(sfx)='KNOLL') substr(adr,length(adr)-len) =' KNL';
    when (strip(sfx)='EXPRESSWAY') substr(adr,length(adr)-len) =' EXPY';
    when (strip(sfx)='SPRNG') substr(adr,length(adr)-len) =' SPG';
    when (strip(sfx)='FLAT') substr(adr,length(adr)-len) =' FLT';
    when (strip(sfx)='GRDEN') substr(adr,length(adr)-len) =' GDN';
    when (strip(sfx)='TRAIL') substr(adr,length(adr)-len) =' TRL';
    when (strip(sfx)='JCTNS') substr(adr,length(adr)-len) =' JCTS';
    when (strip(sfx)='TUNNEL') substr(adr,length(adr)-len) =' TUNL';
    when (strip(sfx)='GROVES') substr(adr,length(adr)-len) =' GRVS';
    when (strip(sfx)='VALLY') substr(adr,length(adr)-len) =' VLY';
    when (strip(sfx)='FERRY') substr(adr,length(adr)-len) =' FRY';
    when (strip(sfx)='PARKWAY') substr(adr,length(adr)-len) =' PKWY';
    when (strip(sfx)='RADIEL') substr(adr,length(adr)-len) =' RADL';
    when (strip(sfx)='STRVNUE') substr(adr,length(adr)-len) =' STRA';
    when (strip(sfx)='OVERPASS') substr(adr,length(adr)-len) =' OPAS';
    when (strip(sfx)='PLAZA') substr(adr,length(adr)-len) =' PLZ';
    when (strip(sfx)='ESTATE') substr(adr,length(adr)-len) =' EST';
    when (strip(sfx)='MNTN') substr(adr,length(adr)-len) =' MTN';
    when (strip(sfx)='LOCK') substr(adr,length(adr)-len) =' LCK';
    when (strip(sfx)='ORCHRD') substr(adr,length(adr)-len) =' ORCH';
    when (strip(sfx)='STRVN') substr(adr,length(adr)-len) =' STRA';
    when (strip(sfx)='LOCKS') substr(adr,length(adr)-len) =' LCKS';
    when (strip(sfx)='BEND') substr(adr,length(adr)-len) =' BND';
    when (strip(sfx)='JUNCTIONS') substr(adr,length(adr)-len) =' JCTS';
    when (strip(sfx)='MOUNTIN') substr(adr,length(adr)-len) =' MTN';
    when (strip(sfx)='BURGS') substr(adr,length(adr)-len) =' BGS';
    when (strip(sfx)='PINE') substr(adr,length(adr)-len) =' PNE';
    when (strip(sfx)='LDGE') substr(adr,length(adr)-len) =' LDG';
    when (strip(sfx)='CAUSWAY') substr(adr,length(adr)-len) =' CSWY';
    when (strip(sfx)='BEACH') substr(adr,length(adr)-len) =' BCH';
    when (strip(sfx)='MOTORWAY') substr(adr,length(adr)-len) =' MTWY';
    when (strip(sfx)='BLUFF') substr(adr,length(adr)-len) =' BLF';
    when (strip(sfx)='COURT') substr(adr,length(adr)-len) =' CT';
    when (strip(sfx)='GROV') substr(adr,length(adr)-len) =' GRV';
    when (strip(sfx)='SPRNGS') substr(adr,length(adr)-len) =' SPGS';
    when (strip(sfx)='OVL') substr(adr,length(adr)-len) =' OVAL';
    when (strip(sfx)='VILLAG') substr(adr,length(adr)-len) =' VLG';
    when (strip(sfx)='VDCT') substr(adr,length(adr)-len) =' VIA';
    when (strip(sfx)='NECK') substr(adr,length(adr)-len) =' NCK';
    when (strip(sfx)='ORCHARD') substr(adr,length(adr)-len) =' ORCH';
    when (strip(sfx)='LIGHT') substr(adr,length(adr)-len) =' LGT';
    when (strip(sfx)='SHORE') substr(adr,length(adr)-len) =' SHR';
    when (strip(sfx)='GREEN') substr(adr,length(adr)-len) =' GRN';
    when (strip(sfx)='ISLND') substr(adr,length(adr)-len) =' IS';
    when (strip(sfx)='TURNPIKE') substr(adr,length(adr)-len) =' TPKE';
    when (strip(sfx)='MISSION') substr(adr,length(adr)-len) =' MSN';
    when (strip(sfx)='SPNGS') substr(adr,length(adr)-len) =' SPGS';
    when (strip(sfx)='COURSE') substr(adr,length(adr)-len) =' CRSE';
    when (strip(sfx)='TRAFFICWAY') substr(adr,length(adr)-len) =' TRFY';
    when (strip(sfx)='TERRACE') substr(adr,length(adr)-len) =' TER';
    when (strip(sfx)='HWAY') substr(adr,length(adr)-len) =' HWY';
    when (strip(sfx)='AVENUE') substr(adr,length(adr)-len) =' AVE';
    when (strip(sfx)='GLEN') substr(adr,length(adr)-len) =' GLN';
    when (strip(sfx)='BOUL') substr(adr,length(adr)-len) =' BLVD';
    when (strip(sfx)='INLET') substr(adr,length(adr)-len) =' INLT';
    when (strip(sfx)='LA') substr(adr,length(adr)-len) =' LN';
    when (strip(sfx)='BROOK') substr(adr,length(adr)-len) =' BRK';
    when (strip(sfx)='SHOAR') substr(adr,length(adr)-len) =' SHR';
    when (strip(sfx)='BYPASS') substr(adr,length(adr)-len) =' BYP';
    when (strip(sfx)='MTIN') substr(adr,length(adr)-len) =' MTN';
    when (strip(sfx)='ALLY') substr(adr,length(adr)-len) =' ALY';
    when (strip(sfx)='FOREST') substr(adr,length(adr)-len) =' FRST';
    when (strip(sfx)='JUNCTION') substr(adr,length(adr)-len) =' JCT';
    when (strip(sfx)='VIEWS') substr(adr,length(adr)-len) =' VWS';
    when (strip(sfx)='WELLS') substr(adr,length(adr)-len) =' WLS';
    when (strip(sfx)='CEN') substr(adr,length(adr)-len) =' CTR';
    when (strip(sfx)='CRT') substr(adr,length(adr)-len) =' CT';
    when (strip(sfx)='CORNERS') substr(adr,length(adr)-len) =' CORS';
    when (strip(sfx)='FRWAY') substr(adr,length(adr)-len) =' FWY';
    when (strip(sfx)='PRARIE') substr(adr,length(adr)-len) =' PR';
    when (strip(sfx)='CROSSING') substr(adr,length(adr)-len) =' XING';
    when (strip(sfx)='EXTN') substr(adr,length(adr)-len) =' EXT';
    when (strip(sfx)='CLIFFS') substr(adr,length(adr)-len) =' CLFS';
    when (strip(sfx)='MANORS') substr(adr,length(adr)-len) =' MNRS';
    when (strip(sfx)='PORTS') substr(adr,length(adr)-len) =' PRTS';
    when (strip(sfx)='GATEWY') substr(adr,length(adr)-len) =' GTWY';
    when (strip(sfx)='SQUARE') substr(adr,length(adr)-len) =' SQ';
    when (strip(sfx)='HARB') substr(adr,length(adr)-len) =' HBR';
    when (strip(sfx)='LOOPS') substr(adr,length(adr)-len) =' LOOP';
    when (strip(sfx)='HILL') substr(adr,length(adr)-len) =' HL';
    when (strip(sfx)='HIGHWAY') substr(adr,length(adr)-len) =' HWY';
    when (strip(sfx)='BROOKS') substr(adr,length(adr)-len) =' BRKS';
    when (strip(sfx)='BRNCH') substr(adr,length(adr)-len) =' BR';
    when (strip(sfx)='AVEN') substr(adr,length(adr)-len) =' AVE';
    when (strip(sfx)='SHORES') substr(adr,length(adr)-len) =' SHRS';
    when (strip(sfx)='ROUTE') substr(adr,length(adr)-len) =' RTE';
    when (strip(sfx)='PLACE') substr(adr,length(adr)-len) =' PL';
    when (strip(sfx)='SUMIT') substr(adr,length(adr)-len) =' SMT';
    when (strip(sfx)='PINES') substr(adr,length(adr)-len) =' PNES';
    when (strip(sfx)='TRKS') substr(adr,length(adr)-len) =' TRAK';
    when (strip(sfx)='SHOAL') substr(adr,length(adr)-len) =' SHL';
    when (strip(sfx)='STRT') substr(adr,length(adr)-len) =' ST';
    when (strip(sfx)='FRWY') substr(adr,length(adr)-len) =' FWY';
    when (strip(sfx)='HEIGHTS') substr(adr,length(adr)-len) =' HTS';
    when (strip(sfx)='RANCHES') substr(adr,length(adr)-len) =' RNCH';
    when (strip(sfx)='BOULEVARD') substr(adr,length(adr)-len) =' BLVD';
    when (strip(sfx)='EXTNSN') substr(adr,length(adr)-len) =' EXT';
    when (strip(sfx)='HOLLOWS') substr(adr,length(adr)-len) =' HOLW';
    when (strip(sfx)='VSTA') substr(adr,length(adr)-len) =' VIS';
    when (strip(sfx)='PLAINS') substr(adr,length(adr)-len) =' PLNS';
    when (strip(sfx)='STATION') substr(adr,length(adr)-len) =' STA';
    when (strip(sfx)='CIRCL') substr(adr,length(adr)-len) =' CIR';
    when (strip(sfx)='MNTNS') substr(adr,length(adr)-len) =' MTNS';
    when (strip(sfx)='VILLAGES') substr(adr,length(adr)-len) =' VLGS';
    when (strip(sfx)='HAVEN') substr(adr,length(adr)-len) =' HVN';
    when (strip(sfx)='TURNPK') substr(adr,length(adr)-len) =' TPKE';
    when (strip(sfx)='EXPR') substr(adr,length(adr)-len) =' EXPY';
    when (strip(sfx)='STN') substr(adr,length(adr)-len) =' STA';
    when (strip(sfx)='EXPW') substr(adr,length(adr)-len) =' EXPY';
    when (strip(sfx)='STREET') substr(adr,length(adr)-len) =' ST';
    when (strip(sfx)='STR') substr(adr,length(adr)-len) =' ST';
    when (strip(sfx)='SPURS') substr(adr,length(adr)-len) =' SPUR';
    when (strip(sfx)='CRECENT') substr(adr,length(adr)-len) =' CRES';
    when (strip(sfx)='RAD') substr(adr,length(adr)-len) =' RADL';
    when (strip(sfx)='RANCH') substr(adr,length(adr)-len) =' RNCH';
    when (strip(sfx)='WELL') substr(adr,length(adr)-len) =' WL';
    when (strip(sfx)='SHOALS') substr(adr,length(adr)-len) =' SHLS';
    when (strip(sfx)='ALLEY') substr(adr,length(adr)-len) =' ALY';
    when (strip(sfx)='PLZA') substr(adr,length(adr)-len) =' PLZ';
    when (strip(sfx)='MEDOWS') substr(adr,length(adr)-len) =' MDWS';
    when (strip(sfx)='ALLEE') substr(adr,length(adr)-len) =' ALY';
    when (strip(sfx)='HAVN') substr(adr,length(adr)-len) =' HVN';
    when (strip(sfx)='PATHS') substr(adr,length(adr)-len) =' PATH';
    when (strip(sfx)='BYPA') substr(adr,length(adr)-len) =' BYP';
    when (strip(sfx)='MILLS') substr(adr,length(adr)-len) =' MLS';
    when (strip(sfx)='PARKS') substr(adr,length(adr)-len) =' PARK';
    when (strip(sfx)='BYPS') substr(adr,length(adr)-len) =' BYP';
    when (strip(sfx)='TUNNELS') substr(adr,length(adr)-len) =' TUNL';
    when (strip(sfx)='CLUB') substr(adr,length(adr)-len) =' CLB';
    when (strip(sfx)='SQRS') substr(adr,length(adr)-len) =' SQS';
    when (strip(sfx)='HLLW') substr(adr,length(adr)-len) =' HOLW';
    when (strip(sfx)='MANOR') substr(adr,length(adr)-len) =' MNR';
    when (strip(sfx)='CENTRE') substr(adr,length(adr)-len) =' CTR';
    when (strip(sfx)='TRACK') substr(adr,length(adr)-len) =' TRAK';
    when (strip(sfx)='HGTS') substr(adr,length(adr)-len) =' HTS';
    when (strip(sfx)='CRCLE') substr(adr,length(adr)-len) =' CIR';
    when (strip(sfx)='FALLS') substr(adr,length(adr)-len) =' FLS';
    when (strip(sfx)='LANDING') substr(adr,length(adr)-len) =' LNDG';
    when (strip(sfx)='PLAINES') substr(adr,length(adr)-len) =' PLNS';
    when (strip(sfx)='VIADCT') substr(adr,length(adr)-len) =' VIA';
    when (strip(sfx)='GROVE') substr(adr,length(adr)-len) =' GRV';
    when (strip(sfx)='CAMP') substr(adr,length(adr)-len) =' CP';
    when (strip(sfx)='TPK') substr(adr,length(adr)-len) =' TPKE';
    when (strip(sfx)='DRIVE') substr(adr,length(adr)-len) =' DR';
    when (strip(sfx)='FREEWAY') substr(adr,length(adr)-len) =' FWY';
    when (strip(sfx)='POINTS') substr(adr,length(adr)-len) =' PTS';
    when (strip(sfx)='EXP') substr(adr,length(adr)-len) =' EXPY';
    when (strip(sfx)='COURTS') substr(adr,length(adr)-len) =' CTS';
    when (strip(sfx)='PKY') substr(adr,length(adr)-len) =' PKWY';
    when (strip(sfx)='CORNER') substr(adr,length(adr)-len) =' COR';
    when (strip(sfx)='CRSSING') substr(adr,length(adr)-len) =' XING';
    when (strip(sfx)='UNIONS') substr(adr,length(adr)-len) =' UNS';
    when (strip(sfx)='LODGE') substr(adr,length(adr)-len) =' LDG';
    when (strip(sfx)='CIRCLE') substr(adr,length(adr)-len) =' CIR';
    when (strip(sfx)='BRIDGE') substr(adr,length(adr)-len) =' BRG';
    when (strip(sfx)='EXPRESS') substr(adr,length(adr)-len) =' EXPY';
    when (strip(sfx)='TUNLS') substr(adr,length(adr)-len) =' TUNL';
    when (strip(sfx)='KNOLLS') substr(adr,length(adr)-len) =' KNLS';
    when (strip(sfx)='GREENS') substr(adr,length(adr)-len) =' GRNS';
    when (strip(sfx)='TUNEL') substr(adr,length(adr)-len) =' TUNL';
    when (strip(sfx)='FIELDS') substr(adr,length(adr)-len) =' FLDS';
    when (strip(sfx)='COMMON') substr(adr,length(adr)-len) =' CMN';
    when (strip(sfx)='RIVER') substr(adr,length(adr)-len) =' RIV';
    when (strip(sfx)='VIEW') substr(adr,length(adr)-len) =' VW';
    when (strip(sfx)='CRSENT') substr(adr,length(adr)-len) =' CRES';
    when (strip(sfx)='RNCHS') substr(adr,length(adr)-len) =' RNCH';
    when (strip(sfx)='CRSCNT') substr(adr,length(adr)-len) =' CRES';
    when (strip(sfx)='RDGE') substr(adr,length(adr)-len) =' RDG';
    when (strip(sfx)='CAUSEWAY') substr(adr,length(adr)-len) =' CSWY';
    when (strip(sfx)='PARKWY') substr(adr,length(adr)-len) =' PKWY';
    when (strip(sfx)='JUNCTON') substr(adr,length(adr)-len) =' JCT';
    when (strip(sfx)='STATN') substr(adr,length(adr)-len) =' STA';
    when (strip(sfx)='GARDN') substr(adr,length(adr)-len) =' GDN';
    when (strip(sfx)='MNTAIN') substr(adr,length(adr)-len) =' MTN';
    when (strip(sfx)='CRSSNG') substr(adr,length(adr)-len) =' XING';
    when (strip(sfx)='RAPID') substr(adr,length(adr)-len) =' RPD';
    when (strip(sfx)='KEY') substr(adr,length(adr)-len) =' KY';
    when (strip(sfx)='WY') substr(adr,length(adr)-len) =' WAY';
    when (strip(sfx)='THROUGHWAY') substr(adr,length(adr)-len) =' TRWY';
    when (strip(sfx)='ESTATES') substr(adr,length(adr)-len) =' ESTS';
    when (strip(sfx)='CK') substr(adr,length(adr)-len) =' CRK';
    when (strip(sfx)='LOAF') substr(adr,length(adr)-len) =' LF';
    when (strip(sfx)='HOLLOW') substr(adr,length(adr)-len) =' HOLW';
    when (strip(sfx)='CANYON') substr(adr,length(adr)-len) =' CYN';
    when (strip(sfx)='VILLAGE') substr(adr,length(adr)-len) =' VLG';
    when (strip(sfx)='CR') substr(adr,length(adr)-len) =' CRK';
    when (strip(sfx)='CT') substr(adr,length(adr)-len) =' CTS';
    when (strip(sfx)='JCTION') substr(adr,length(adr)-len) =' JCT';
    when (strip(sfx)='MSSN') substr(adr,length(adr)-len) =' MSN';
    when (strip(sfx)='BRDGE') substr(adr,length(adr)-len) =' BRG';
    when (strip(sfx)='CENT') substr(adr,length(adr)-len) =' CTR';
    when (strip(sfx)='FRT') substr(adr,length(adr)-len) =' FT';
    when (strip(sfx)='PK') substr(adr,length(adr)-len) =' PARK';
    when (strip(sfx)='LANES') substr(adr,length(adr)-len) =' LN';
    when (strip(sfx)='GTWAY') substr(adr,length(adr)-len) =' GTWY';
    when (strip(sfx)='PRK') substr(adr,length(adr)-len) =' PARK';
    when (strip(sfx)='STRAVENUE') substr(adr,length(adr)-len) =' STRA';
    when (strip(sfx)='HIWAY') substr(adr,length(adr)-len) =' HWY';
    when (strip(sfx)='VILLE') substr(adr,length(adr)-len) =' VL';
    when (strip(sfx)='PLAIN') substr(adr,length(adr)-len) =' PLN';
    when (strip(sfx)='MOUNT') substr(adr,length(adr)-len) =' MT';
    when (strip(sfx)='CENTR') substr(adr,length(adr)-len) =' CTR';
    when (strip(sfx)='PRR') substr(adr,length(adr)-len) =' PR';
    when (strip(sfx)='AVN') substr(adr,length(adr)-len) =' AVE';
    when (strip(sfx)='SPNG') substr(adr,length(adr)-len) =' SPG';
    when (strip(sfx)='HIWY') substr(adr,length(adr)-len) =' HWY';
    when (strip(sfx)='DAM') substr(adr,length(adr)-len) =' DM';
    when (strip(sfx)='CRCL') substr(adr,length(adr)-len) =' CIR';
    when (strip(sfx)='SQRE') substr(adr,length(adr)-len) =' SQ';
    when (strip(sfx)='JCTN') substr(adr,length(adr)-len) =' JCT';
    when (strip(sfx)='MOUNTAIN') substr(adr,length(adr)-len) =' MTN';
    when (strip(sfx)='KEYS') substr(adr,length(adr)-len) =' KYS';
    when (strip(sfx)='PARKWAYS') substr(adr,length(adr)-len) =' PKWY';
    when (strip(sfx)='DRIVES') substr(adr,length(adr)-len) =' DRS';
    when (strip(sfx)='CENTER') substr(adr,length(adr)-len) =' CTR';
    when (strip(sfx)='DRIV') substr(adr,length(adr)-len) =' DR';
    when (strip(sfx)='SUMITT') substr(adr,length(adr)-len) =' SMT';
    when (strip(sfx)='CANYN') substr(adr,length(adr)-len) =' CYN';
    when (strip(sfx)='HARBR') substr(adr,length(adr)-len) =' HBR';
    when (strip(sfx)='REST') substr(adr,length(adr)-len) =' RST';
    when (strip(sfx)='SHOARS') substr(adr,length(adr)-len) =' SHRS';
    when (strip(sfx)='VIST') substr(adr,length(adr)-len) =' VIS';
    when (strip(sfx)='ISLNDS') substr(adr,length(adr)-len) =' ISS';
    when (strip(sfx)='HILLS') substr(adr,length(adr)-len) =' HLS';
    when (strip(sfx)='CRESENT') substr(adr,length(adr)-len) =' CRES';
    when (strip(sfx)='POINT') substr(adr,length(adr)-len) =' PT';
    when (strip(sfx)='LAKE') substr(adr,length(adr)-len) =' LK';
    when (strip(sfx)='VLLY') substr(adr,length(adr)-len) =' VLY';
    when (strip(sfx)='STRAV') substr(adr,length(adr)-len) =' STRA';
    when (strip(sfx)='CROSSROAD') substr(adr,length(adr)-len) =' XRD';
    when (strip(sfx)='STRAVE') substr(adr,length(adr)-len) =' STRA';
    when (strip(sfx)='STRAVN') substr(adr,length(adr)-len) =' STRA';
    when (strip(sfx)='KNOL') substr(adr,length(adr)-len) =' KNL';
    when (strip(sfx)='FORGE') substr(adr,length(adr)-len) =' FRG';
    when (strip(sfx)='CNTR') substr(adr,length(adr)-len) =' CTR';
    when (strip(sfx)='CAPE') substr(adr,length(adr)-len) =' CPE';
    when (strip(sfx)='HEIGHT') substr(adr,length(adr)-len) =' HTS';
    when (strip(sfx)='HIGHWY') substr(adr,length(adr)-len) =' HWY';
    when (strip(sfx)='TRNPK') substr(adr,length(adr)-len) =' TPKE';
    when (strip(sfx)='BOULV') substr(adr,length(adr)-len) =' BLVD';
    when (strip(sfx)='CIRCLES') substr(adr,length(adr)-len) =' CIRS';
    when (strip(sfx)='VALLEYS') substr(adr,length(adr)-len) =' VLYS';
    when (strip(sfx)='VST') substr(adr,length(adr)-len) =' VIS';
    when (strip(sfx)='CREEK') substr(adr,length(adr)-len) =' CRK';
    when (strip(sfx)='SPRING') substr(adr,length(adr)-len) =' SPG';
    when (strip(sfx)='HOLWS') substr(adr,length(adr)-len) =' HOLW';
    when (strip(sfx)='TRACE') substr(adr,length(adr)-len) =' TRCE';
    when (strip(sfx)='BOTTOM') substr(adr,length(adr)-len) =' BTM';
    when (strip(sfx)='STREME') substr(adr,length(adr)-len) =' STRM';
    when (strip(sfx)='ISLES') substr(adr,length(adr)-len) =' ISLE';
    when (strip(sfx)='CIRC') substr(adr,length(adr)-len) =' CIR';
    when (strip(sfx)='FORKS') substr(adr,length(adr)-len) =' FRKS';
    when (strip(sfx)='BURG') substr(adr,length(adr)-len) =' BG';
    when (strip(sfx)='TRLS') substr(adr,length(adr)-len) =' TRL';
    when (strip(sfx)='RADIAL') substr(adr,length(adr)-len) =' RADL';
    when (strip(sfx)='LAKES') substr(adr,length(adr)-len) =' LKS';
    when (strip(sfx)='EXTENSION') substr(adr,length(adr)-len) =' EXT';
    when (strip(sfx)='ISLAND') substr(adr,length(adr)-len) =' IS';
    when (strip(sfx)='TERR') substr(adr,length(adr)-len) =' TER';
    when (strip(sfx)='UNION') substr(adr,length(adr)-len) =' UN';
    when (strip(sfx)='EXTENSIONS') substr(adr,length(adr)-len) =' EXTS';
    when (strip(sfx)='PKWYS') substr(adr,length(adr)-len) =' PKWY';
    when (strip(sfx)='ISLANDS') substr(adr,length(adr)-len) =' ISS';
    when (strip(sfx)='ROAD') substr(adr,length(adr)-len) =' RD';
    when (strip(sfx)='ROADS') substr(adr,length(adr)-len) =' RDS';
    when (strip(sfx)='GLENS') substr(adr,length(adr)-len) =' GLNS';
    when (strip(sfx)='SPRINGS') substr(adr,length(adr)-len) =' SPGS';
    when (strip(sfx)='MISSN') substr(adr,length(adr)-len) =' MSN';
    when (strip(sfx)='RIDGE') substr(adr,length(adr)-len) =' RDG';
    when (strip(sfx)='ARCADE') substr(adr,length(adr)-len) =' ARC';
    when (strip(sfx)='BAYOU') substr(adr,length(adr)-len) =' BYU';
    when (strip(sfx)='CRSNT') substr(adr,length(adr)-len) =' CRES';
    when (strip(sfx)='JUNCTN') substr(adr,length(adr)-len) =' JCT';
    when (strip(sfx)='VALLEY') substr(adr,length(adr)-len) =' VLY';
    when (strip(sfx)='FORK') substr(adr,length(adr)-len) =' FRK';
    when (strip(sfx)='MOUNTAINS') substr(adr,length(adr)-len) =' MTNS';
    when (strip(sfx)='BOTTM') substr(adr,length(adr)-len) =' BTM';
    when (strip(sfx)='FORG') substr(adr,length(adr)-len) =' FRG';
    when (strip(sfx)='HT') substr(adr,length(adr)-len) =' HTS';
    when (strip(sfx)='FORD') substr(adr,length(adr)-len) =' FRD';
    when (strip(sfx)='GRDN') substr(adr,length(adr)-len) =' GDN';
    when (strip(sfx)='FORT') substr(adr,length(adr)-len) =' FT';
    when (strip(sfx)='TRACES') substr(adr,length(adr)-len) =' TRCE';
    when (strip(sfx)='CNYN') substr(adr,length(adr)-len) =' CYN';
    when (strip(sfx)='FLATS') substr(adr,length(adr)-len) =' FLTS';
    when (strip(sfx)='ANEX') substr(adr,length(adr)-len) =' ANX';
    when (strip(sfx)='GATWAY') substr(adr,length(adr)-len) =' GTWY';
    when (strip(sfx)='RAPIDS') substr(adr,length(adr)-len) =' RPDS';
    when (strip(sfx)='VILLIAGE') substr(adr,length(adr)-len) =' VLG';
    when (strip(sfx)='COVES') substr(adr,length(adr)-len) =' CVS';
    when (strip(sfx)='RVR') substr(adr,length(adr)-len) =' RIV';
    when (strip(sfx)='AV') substr(adr,length(adr)-len) =' AVE';
    when (strip(sfx)='PIKES') substr(adr,length(adr)-len) =' PIKE';
    when (strip(sfx)='VISTA') substr(adr,length(adr)-len) =' VIS';
    when (strip(sfx)='FORESTS') substr(adr,length(adr)-len) =' FRST';
    when (strip(sfx)='FIELD') substr(adr,length(adr)-len) =' FLD';
    when (strip(sfx)='BRANCH') substr(adr,length(adr)-len) =' BR';
    when (strip(sfx)='DALE') substr(adr,length(adr)-len) =' DL';
    when (strip(sfx)='ANNEX') substr(adr,length(adr)-len) =' ANX';
    when (strip(sfx)='SQR') substr(adr,length(adr)-len) =' SQ';
    when (strip(sfx)='COVE') substr(adr,length(adr)-len) =' CV';
    when (strip(sfx)='SQU') substr(adr,length(adr)-len) =' SQ';
    when (strip(sfx)='SKYWAY') substr(adr,length(adr)-len) =' SKWY';
    when (strip(sfx)='RIDGES') substr(adr,length(adr)-len) =' RDGS';
    when (strip(sfx)='TUNNL') substr(adr,length(adr)-len) =' TUNL';
    when (strip(sfx)='UNDERPASS') substr(adr,length(adr)-len) =' UPAS';
    when (strip(sfx)='CLIFF') substr(adr,length(adr)-len) =' CLF';
    when (strip(sfx)='LANE') substr(adr,length(adr)-len) =' LN';
    when (strip(sfx)='DVD') substr(adr,length(adr)-len) =' DV';
    when (strip(sfx)='CURVE') substr(adr,length(adr)-len) =' CURV';
    when (strip(sfx)='SUMMIT') substr(adr,length(adr)-len) =' SMT';
    when (strip(sfx)='GARDENS') substr(adr,length(adr)-len) =' GDNS';
    ;;;;
    run;quit;

    /*       _                        _                         _       _                  _               _
    (_)_ __ | |_ ___ _ __ _ __   __ _| | __      _____  _ __ __| |  ___| |_ __ _ _ __   __| | __ _ _ __ __| |
    | | `_ \| __/ _ \ `__| `_ \ / _` | | \ \ /\ / / _ \| `__/ _` | / __| __/ _` | `_ \ / _` |/ _` | `__/ _` |
    | | | | | ||  __/ |  | | | | (_| | |  \ V  V / (_) | | | (_| | \__ \ || (_| | | | | (_| | (_| | | | (_| |
    |_|_| |_|\__\___|_|  |_| |_|\__,_|_|   \_/\_/ \___/|_|  \__,_| |___/\__\__,_|_| |_|\__,_|\__,_|_|  \__,_|

    */

    filename ft15f001 clear;
    filename ft15f001 "%sysfunc(pathname(work))/adr_010sx1.sas";
    parmcards4;
         select;
            when (index(adr,' STREET '))      adr=tranwrd(adr,' STREET '     , ' ST '       );
            when (index(adr,' AVENUE '))      adr=tranwrd(adr,' AVENUE '     , ' AVE '      );
            when (index(adr,' ROAD '))        adr=tranwrd(adr,' ROAD '       , ' RD '       );
            when (index(adr,' BOULEVARD '))   adr=tranwrd(adr,' BOULEVARD '  , ' BLVD '     );
            when (index(adr,' VALLEY '))      adr=tranwrd(adr,' VALLEY '     , ' VLY '      );
            when (index(adr,' CNTER '))       adr=tranwrd(adr,' CNTER '      , ' CTR '      );
            when (index(adr,' ROUTE '))       adr=tranwrd(adr,' ROUTE '      , ' RTE '      );
            when (index(adr,' HEIGHTS '))     adr=tranwrd(adr,' HEIGHTS '    , ' HTS '      );
            when (index(adr,' EXPRESSWAY '))  adr=tranwrd(adr,' EXPRESSWAY ' , ' EXPY '     );
            when (index(adr,' CREEK '))       adr=tranwrd(adr,' CREEK '      , ' CRK '      );
            when (index(adr,' CIRCL '))       adr=tranwrd(adr,' CIRCL '      , ' CIR '      );
            when (index(adr,' TRPK '))        adr=tranwrd(adr,' TRPK '       , ' TPKE '     );
            when (index(adr,' PKY '))         adr=tranwrd(adr,' PKY '        , ' PKWY '     );
            when (index(adr,' FORGES '))      adr=tranwrd(adr,' FORGES '     , ' FRGS '     );
            when (index(adr,' BYPAS '))       adr=tranwrd(adr,' BYPAS '      , ' BYP '      );
            when (index(adr,' VIADUCT '))     adr=tranwrd(adr,' VIADUCT '    , ' VIA '      );
            when (index(adr,' MNT '))         adr=tranwrd(adr,' MNT '        , ' MT '       );
            when (index(adr,' LNDNG '))       adr=tranwrd(adr,' LNDNG '      , ' LNDG '     );
            when (index(adr,' VILL '))        adr=tranwrd(adr,' VILL '       , ' VLG '      );
            when (index(adr,' MILL '))        adr=tranwrd(adr,' MILL '       , ' ML '       );
            when (index(adr,' CENTERS '))     adr=tranwrd(adr,' CENTERS '    , ' CTRS '     );
            when (index(adr,' HRBOR '))       adr=tranwrd(adr,' HRBOR '      , ' HBR '      );
            when (index(adr,' TR '))          adr=tranwrd(adr,' TR '         , ' TRL '      );
            when (index(adr,' PASSAGE '))     adr=tranwrd(adr,' PASSAGE '    , ' PSGE '     );
            when (index(adr,' WALKS '))       adr=tranwrd(adr,' WALKS '      , ' WALK '     );
            when (index(adr,' CREST '))       adr=tranwrd(adr,' CREST '      , ' CRST '     );
            when (index(adr,' MEADOWS '))     adr=tranwrd(adr,' MEADOWS '    , ' MDWS '     );
            when (index(adr,' FREEWY '))      adr=tranwrd(adr,' FREEWY '     , ' FWY '      );
            when (index(adr,' GARDEN '))      adr=tranwrd(adr,' GARDEN '     , ' GDN '      );
            when (index(adr,' BLUFFS '))      adr=tranwrd(adr,' BLUFFS '     , ' BLFS '     );
            when (index(adr,' TRK '))         adr=tranwrd(adr,' TRK '        , ' TRAK '     );
            when (index(adr,' SQUARES '))     adr=tranwrd(adr,' SQUARES '    , ' SQS '      );
            when (index(adr,' HARBOR '))      adr=tranwrd(adr,' HARBOR '     , ' HBR '      );
            when (index(adr,' FRRY '))        adr=tranwrd(adr,' FRRY '       , ' FRY '      );
            when (index(adr,' DIV '))         adr=tranwrd(adr,' DIV '        , ' DV '       );
            when (index(adr,' STRAVEN '))     adr=tranwrd(adr,' STRAVEN '    , ' STRA '     );
            when (index(adr,' CMP '))         adr=tranwrd(adr,' CMP '        , ' CP '       );
            when (index(adr,' GRDNS '))       adr=tranwrd(adr,' GRDNS '      , ' GDNS '     );
            when (index(adr,' VILLG '))       adr=tranwrd(adr,' VILLG '      , ' VLG '      );
            when (index(adr,' MEADOW '))      adr=tranwrd(adr,' MEADOW '     , ' MDW '      );
            when (index(adr,' TRAILS '))      adr=tranwrd(adr,' TRAILS '     , ' TRL '      );
            when (index(adr,' STREETS '))     adr=tranwrd(adr,' STREETS '    , ' STS '      );
            when (index(adr,' PRAIRIE '))     adr=tranwrd(adr,' PRAIRIE '    , ' PR '       );
            when (index(adr,' CRESCENT '))    adr=tranwrd(adr,' CRESCENT '   , ' CRES '     );
            when (index(adr,' PORT '))        adr=tranwrd(adr,' PORT '       , ' PRT '      );
            when (index(adr,' TRAFFICWAY '))  adr=tranwrd(adr,' TRAFFICWAY ' , ' TRFY '     );
            when (index(adr,' BLUF '))        adr=tranwrd(adr,' BLUF '       , ' BLF '      );
            when (index(adr,' AVNUE '))       adr=tranwrd(adr,' AVNUE '      , ' AVE '      );
            when (index(adr,' LIGHTS '))      adr=tranwrd(adr,' LIGHTS '     , ' LGTS '     );
            when (index(adr,' HARBORS '))     adr=tranwrd(adr,' HARBORS '    , ' HBRS '     );
            when (index(adr,' LODG '))        adr=tranwrd(adr,' LODG '       , ' LDG '      );
            when (index(adr,' TRACKS '))      adr=tranwrd(adr,' TRACKS '     , ' TRAK '     );
            when (index(adr,' PKWAY '))       adr=tranwrd(adr,' PKWAY '      , ' PKWY '     );
            when (index(adr,' BOT '))         adr=tranwrd(adr,' BOT '        , ' BTM '      );
            when (index(adr,' DRV '))         adr=tranwrd(adr,' DRV '        , ' DR '       );
            when (index(adr,' DIVIDE '))      adr=tranwrd(adr,' DIVIDE '     , ' DV '       );
            when (index(adr,' FORDS '))       adr=tranwrd(adr,' FORDS '      , ' FRDS '     );
            when (index(adr,' AVENU '))       adr=tranwrd(adr,' AVENU '      , ' AVE '      );
            when (index(adr,' RIVR '))        adr=tranwrd(adr,' RIVR '       , ' RIV '      );
            when (index(adr,' GATEWAY '))     adr=tranwrd(adr,' GATEWAY '    , ' GTWY '     );
            when (index(adr,' STREAM '))      adr=tranwrd(adr,' STREAM '     , ' STRM '     );
            when (index(adr,' BAYOO '))       adr=tranwrd(adr,' BAYOO '      , ' BYU '      );
            when (index(adr,' KNOLL '))       adr=tranwrd(adr,' KNOLL '      , ' KNL '      );
            when (index(adr,' SPRNG '))       adr=tranwrd(adr,' SPRNG '      , ' SPG '      );
            when (index(adr,' FLAT '))        adr=tranwrd(adr,' FLAT '       , ' FLT '      );
            when (index(adr,' GRDEN '))       adr=tranwrd(adr,' GRDEN '      , ' GDN '      );
            when (index(adr,' TRAIL '))       adr=tranwrd(adr,' TRAIL '      , ' TRL '      );
            when (index(adr,' JCTNS '))       adr=tranwrd(adr,' JCTNS '      , ' JCTS '     );
            when (index(adr,' TUNNEL '))      adr=tranwrd(adr,' TUNNEL '     , ' TUNL '     );
            when (index(adr,' GROVES '))      adr=tranwrd(adr,' GROVES '     , ' GRVS '     );
            when (index(adr,' VALLY '))       adr=tranwrd(adr,' VALLY '      , ' VLY '      );
            when (index(adr,' FERRY '))       adr=tranwrd(adr,' FERRY '      , ' FRY '      );
            when (index(adr,' PARKWAY '))     adr=tranwrd(adr,' PARKWAY '    , ' PKWY '     );
            when (index(adr,' RADIEL '))      adr=tranwrd(adr,' RADIEL '     , ' RADL '     );
            when (index(adr,' STRVNUE '))     adr=tranwrd(adr,' STRVNUE '    , ' STRA '     );
            when (index(adr,' OVERPASS '))    adr=tranwrd(adr,' OVERPASS '   , ' OPAS '     );
            when (index(adr,' PLAZA '))       adr=tranwrd(adr,' PLAZA '      , ' PLZ '      );
            when (index(adr,' ESTATE '))      adr=tranwrd(adr,' ESTATE '     , ' EST '      );
            when (index(adr,' MNTN '))        adr=tranwrd(adr,' MNTN '       , ' MTN '      );
            when (index(adr,' LOCK '))        adr=tranwrd(adr,' LOCK '       , ' LCK '      );
            when (index(adr,' ORCHRD '))      adr=tranwrd(adr,' ORCHRD '     , ' ORCH '     );
            when (index(adr,' STRVN '))       adr=tranwrd(adr,' STRVN '      , ' STRA '     );
            when (index(adr,' LOCKS '))       adr=tranwrd(adr,' LOCKS '      , ' LCKS '     );
            when (index(adr,' BEND '))        adr=tranwrd(adr,' BEND '       , ' BND '      );
            when (index(adr,' JUNCTIONS '))   adr=tranwrd(adr,' JUNCTIONS '  , ' JCTS '     );
            when (index(adr,' MOUNTIN '))     adr=tranwrd(adr,' MOUNTIN '    , ' MTN '      );
            when (index(adr,' BURGS '))       adr=tranwrd(adr,' BURGS '      , ' BGS '      );
            when (index(adr,' PINE '))        adr=tranwrd(adr,' PINE '       , ' PNE '      );
            when (index(adr,' LDGE '))        adr=tranwrd(adr,' LDGE '       , ' LDG '      );
            when (index(adr,' CAUSWAY '))     adr=tranwrd(adr,' CAUSWAY '    , ' CSWY '     );
            when (index(adr,' BEACH '))       adr=tranwrd(adr,' BEACH '      , ' BCH '      );
            when (index(adr,' MOTORWAY '))    adr=tranwrd(adr,' MOTORWAY '   , ' MTWY '     );
            when (index(adr,' BLUFF '))       adr=tranwrd(adr,' BLUFF '      , ' BLF '      );
            when (index(adr,' COURT '))       adr=tranwrd(adr,' COURT '      , ' CT '       );
            when (index(adr,' GROV '))        adr=tranwrd(adr,' GROV '       , ' GRV '      );
            when (index(adr,' SPRNGS '))      adr=tranwrd(adr,' SPRNGS '     , ' SPGS '     );
            when (index(adr,' OVL '))         adr=tranwrd(adr,' OVL '        , ' OVAL '     );
            when (index(adr,' VILLAG '))      adr=tranwrd(adr,' VILLAG '     , ' VLG '      );
            when (index(adr,' VDCT '))        adr=tranwrd(adr,' VDCT '       , ' VIA '      );
            when (index(adr,' NECK '))        adr=tranwrd(adr,' NECK '       , ' NCK '      );
            when (index(adr,' ORCHARD '))     adr=tranwrd(adr,' ORCHARD '    , ' ORCH '     );
            when (index(adr,' LIGHT '))       adr=tranwrd(adr,' LIGHT '      , ' LGT '      );
            when (index(adr,' SHORE '))       adr=tranwrd(adr,' SHORE '      , ' SHR '      );
            when (index(adr,' GREEN '))       adr=tranwrd(adr,' GREEN '      , ' GRN '      );
            when (index(adr,' ISLND '))       adr=tranwrd(adr,' ISLND '      , ' IS '       );
            when (index(adr,' TURNPIKE '))    adr=tranwrd(adr,' TURNPIKE '   , ' TPKE '     );
            when (index(adr,' MISSION '))     adr=tranwrd(adr,' MISSION '    , ' MSN '      );
            when (index(adr,' SPNGS '))       adr=tranwrd(adr,' SPNGS '      , ' SPGS '     );
            when (index(adr,' COURSE '))      adr=tranwrd(adr,' COURSE '     , ' CRSE '     );
            when (index(adr,' TERRACE '))     adr=tranwrd(adr,' TERRACE '    , ' TER '      );
            when (index(adr,' HWAY '))        adr=tranwrd(adr,' HWAY '       , ' HWY '      );
            when (index(adr,' GLEN '))        adr=tranwrd(adr,' GLEN '       , ' GLN '      );
            when (index(adr,' BOUL '))        adr=tranwrd(adr,' BOUL '       , ' BLVD '     );
            when (index(adr,' INLET '))       adr=tranwrd(adr,' INLET '      , ' INLT '     );
            when (index(adr,' LA '))          adr=tranwrd(adr,' LA '         , ' LN '       );
            when (index(adr,' BROOK '))       adr=tranwrd(adr,' BROOK '      , ' BRK '      );
            when (index(adr,' SHOAR '))       adr=tranwrd(adr,' SHOAR '      , ' SHR '      );
            when (index(adr,' BYPASS '))      adr=tranwrd(adr,' BYPASS '     , ' BYP '      );
            when (index(adr,' MTIN '))        adr=tranwrd(adr,' MTIN '       , ' MTN '      );
            when (index(adr,' ALLY '))        adr=tranwrd(adr,' ALLY '       , ' ALY '      );
            when (index(adr,' FOREST '))      adr=tranwrd(adr,' FOREST '     , ' FRST '     );
            when (index(adr,' JUNCTION '))    adr=tranwrd(adr,' JUNCTION '   , ' JCT '      );
            when (index(adr,' VIEWS '))       adr=tranwrd(adr,' VIEWS '      , ' VWS '      );
            when (index(adr,' WELLS '))       adr=tranwrd(adr,' WELLS '      , ' WLS '      );
            when (index(adr,' CEN '))         adr=tranwrd(adr,' CEN '        , ' CTR '      );
            when (index(adr,' CRT '))         adr=tranwrd(adr,' CRT '        , ' CT '       );
            when (index(adr,' CORNERS '))     adr=tranwrd(adr,' CORNERS '    , ' CORS '     );
            when (index(adr,' FRWAY '))       adr=tranwrd(adr,' FRWAY '      , ' FWY '      );
            when (index(adr,' PRARIE '))      adr=tranwrd(adr,' PRARIE '     , ' PR '       );
            when (index(adr,' CROSSING '))    adr=tranwrd(adr,' CROSSING '   , ' XING '     );
            when (index(adr,' EXTN '))        adr=tranwrd(adr,' EXTN '       , ' EXT '      );
            when (index(adr,' CLIFFS '))      adr=tranwrd(adr,' CLIFFS '     , ' CLFS '     );
            when (index(adr,' MANORS '))      adr=tranwrd(adr,' MANORS '     , ' MNRS '     );
            when (index(adr,' PORTS '))       adr=tranwrd(adr,' PORTS '      , ' PRTS '     );
            when (index(adr,' GATEWY '))      adr=tranwrd(adr,' GATEWY '     , ' GTWY '     );
            when (index(adr,' SQUARE '))      adr=tranwrd(adr,' SQUARE '     , ' SQ '       );
            when (index(adr,' HARB '))        adr=tranwrd(adr,' HARB '       , ' HBR '      );
            when (index(adr,' LOOPS '))       adr=tranwrd(adr,' LOOPS '      , ' LOOP '     );
            when (index(adr,' HILL '))        adr=tranwrd(adr,' HILL '       , ' HL '       );
            when (index(adr,' HIGHWAY '))     adr=tranwrd(adr,' HIGHWAY '    , ' HWY '      );
            when (index(adr,' BROOKS '))      adr=tranwrd(adr,' BROOKS '     , ' BRKS '     );
            when (index(adr,' BRNCH '))       adr=tranwrd(adr,' BRNCH '      , ' BR '       );
            when (index(adr,' AVEN '))        adr=tranwrd(adr,' AVEN '       , ' AVE '      );
            when (index(adr,' SHORES '))      adr=tranwrd(adr,' SHORES '     , ' SHRS '     );
            when (index(adr,' PLACE '))       adr=tranwrd(adr,' PLACE '      , ' PL '       );
            when (index(adr,' SUMIT '))       adr=tranwrd(adr,' SUMIT '      , ' SMT '      );
            when (index(adr,' PINES '))       adr=tranwrd(adr,' PINES '      , ' PNES '     );
            when (index(adr,' TRKS '))        adr=tranwrd(adr,' TRKS '       , ' TRAK '     );
            when (index(adr,' SHOAL '))       adr=tranwrd(adr,' SHOAL '      , ' SHL '      );
            when (index(adr,' STRT '))        adr=tranwrd(adr,' STRT '       , ' ST '       );
            when (index(adr,' FRWY '))        adr=tranwrd(adr,' FRWY '       , ' FWY '      );
            when (index(adr,' RANCHES '))     adr=tranwrd(adr,' RANCHES '    , ' RNCH '     );
            when (index(adr,' BOULEVARD '))   adr=tranwrd(adr,' BOULEVARD '  , ' BLVD '     );
            when (index(adr,' EXTNSN '))      adr=tranwrd(adr,' EXTNSN '     , ' EXT '      );
            when (index(adr,' HOLLOWS '))     adr=tranwrd(adr,' HOLLOWS '    , ' HOLW '     );
            when (index(adr,' VSTA '))        adr=tranwrd(adr,' VSTA '       , ' VIS '      );
            when (index(adr,' PLAINS '))      adr=tranwrd(adr,' PLAINS '     , ' PLNS '     );
            when (index(adr,' STATION '))     adr=tranwrd(adr,' STATION '    , ' STA '      );
            when (index(adr,' MNTNS '))       adr=tranwrd(adr,' MNTNS '      , ' MTNS '     );
            when (index(adr,' VILLAGES '))    adr=tranwrd(adr,' VILLAGES '   , ' VLGS '     );
            when (index(adr,' HAVEN '))       adr=tranwrd(adr,' HAVEN '      , ' HVN '      );
            when (index(adr,' TURNPK '))      adr=tranwrd(adr,' TURNPK '     , ' TPKE '     );
            when (index(adr,' EXPR '))        adr=tranwrd(adr,' EXPR '       , ' EXPY '     );
            when (index(adr,' STN '))         adr=tranwrd(adr,' STN '        , ' STA '      );
            when (index(adr,' EXPW '))        adr=tranwrd(adr,' EXPW '       , ' EXPY '     );
            when (index(adr,' STR '))         adr=tranwrd(adr,' STR '        , ' ST '       );
            when (index(adr,' SPURS '))       adr=tranwrd(adr,' SPURS '      , ' SPUR '     );
            when (index(adr,' CRECENT '))     adr=tranwrd(adr,' CRECENT '    , ' CRES '     );
            when (index(adr,' RAD '))         adr=tranwrd(adr,' RAD '        , ' RADL '     );
            when (index(adr,' RANCH '))       adr=tranwrd(adr,' RANCH '      , ' RNCH '     );
            when (index(adr,' WELL '))        adr=tranwrd(adr,' WELL '       , ' WL '       );
            when (index(adr,' SHOALS '))      adr=tranwrd(adr,' SHOALS '     , ' SHLS '     );
            when (index(adr,' ALLEY '))       adr=tranwrd(adr,' ALLEY '      , ' ALY '      );
            when (index(adr,' PLZA '))        adr=tranwrd(adr,' PLZA '       , ' PLZ '      );
            when (index(adr,' MEDOWS '))      adr=tranwrd(adr,' MEDOWS '     , ' MDWS '     );
            when (index(adr,' ALLEE '))       adr=tranwrd(adr,' ALLEE '      , ' ALY '      );
            when (index(adr,' HAVN '))        adr=tranwrd(adr,' HAVN '       , ' HVN '      );
            when (index(adr,' PATHS '))       adr=tranwrd(adr,' PATHS '      , ' PATH '     );
            when (index(adr,' BYPA '))        adr=tranwrd(adr,' BYPA '       , ' BYP '      );
            when (index(adr,' MILLS '))       adr=tranwrd(adr,' MILLS '      , ' MLS '      );
            when (index(adr,' PARKS '))       adr=tranwrd(adr,' PARKS '      , ' PARK '     );
            when (index(adr,' BYPS '))        adr=tranwrd(adr,' BYPS '       , ' BYP '      );
            when (index(adr,' TUNNELS '))     adr=tranwrd(adr,' TUNNELS '    , ' TUNL '     );
            when (index(adr,' CLUB '))        adr=tranwrd(adr,' CLUB '       , ' CLB '      );
            when (index(adr,' SQRS '))        adr=tranwrd(adr,' SQRS '       , ' SQS '      );
            when (index(adr,' HLLW '))        adr=tranwrd(adr,' HLLW '       , ' HOLW '     );
            when (index(adr,' MANOR '))       adr=tranwrd(adr,' MANOR '      , ' MNR '      );
            when (index(adr,' CENTRE '))      adr=tranwrd(adr,' CENTRE '     , ' CTR '      );
            when (index(adr,' TRACK '))       adr=tranwrd(adr,' TRACK '      , ' TRAK '     );
            when (index(adr,' HGTS '))        adr=tranwrd(adr,' HGTS '       , ' HTS '      );
            when (index(adr,' CRCLE '))       adr=tranwrd(adr,' CRCLE '      , ' CIR '      );
            when (index(adr,' FALLS '))       adr=tranwrd(adr,' FALLS '      , ' FLS '      );
            when (index(adr,' LANDING '))     adr=tranwrd(adr,' LANDING '    , ' LNDG '     );
            when (index(adr,' PLAINES '))     adr=tranwrd(adr,' PLAINES '    , ' PLNS '     );
            when (index(adr,' VIADCT '))      adr=tranwrd(adr,' VIADCT '     , ' VIA '      );
            when (index(adr,' GROVE '))       adr=tranwrd(adr,' GROVE '      , ' GRV '      );
            when (index(adr,' CAMP '))        adr=tranwrd(adr,' CAMP '       , ' CP '       );
            when (index(adr,' TPK '))         adr=tranwrd(adr,' TPK '        , ' TPKE '     );
            when (index(adr,' DRIVE '))       adr=tranwrd(adr,' DRIVE '      , ' DR '       );
            when (index(adr,' FREEWAY '))     adr=tranwrd(adr,' FREEWAY '    , ' FWY '      );
            when (index(adr,' POINTS '))      adr=tranwrd(adr,' POINTS '     , ' PTS '      );
            when (index(adr,' EXP '))         adr=tranwrd(adr,' EXP '        , ' EXPY '     );
            when (index(adr,' COURTS '))      adr=tranwrd(adr,' COURTS '     , ' CTS '      );
            when (index(adr,' CORNER '))      adr=tranwrd(adr,' CORNER '     , ' COR '      );
            when (index(adr,' CRSSING '))     adr=tranwrd(adr,' CRSSING '    , ' XING '     );
            when (index(adr,' UNIONS '))      adr=tranwrd(adr,' UNIONS '     , ' UNS '      );
            when (index(adr,' LODGE '))       adr=tranwrd(adr,' LODGE '      , ' LDG '      );
            when (index(adr,' CIRCLE '))      adr=tranwrd(adr,' CIRCLE '     , ' CIR '      );
            when (index(adr,' BRIDGE '))      adr=tranwrd(adr,' BRIDGE '     , ' BRG '      );
            when (index(adr,' EXPRESS '))     adr=tranwrd(adr,' EXPRESS '    , ' EXPY '     );
            when (index(adr,' TUNLS '))       adr=tranwrd(adr,' TUNLS '      , ' TUNL '     );
            when (index(adr,' KNOLLS '))      adr=tranwrd(adr,' KNOLLS '     , ' KNLS '     );
            when (index(adr,' GREENS '))      adr=tranwrd(adr,' GREENS '     , ' GRNS '     );
            when (index(adr,' TUNEL '))       adr=tranwrd(adr,' TUNEL '      , ' TUNL '     );
            when (index(adr,' FIELDS '))      adr=tranwrd(adr,' FIELDS '     , ' FLDS '     );
            when (index(adr,' COMMON '))      adr=tranwrd(adr,' COMMON '     , ' CMN '      );
            when (index(adr,' RIVER '))       adr=tranwrd(adr,' RIVER '      , ' RIV '      );
            when (index(adr,' VIEW '))        adr=tranwrd(adr,' VIEW '       , ' VW '       );
            when (index(adr,' CRSENT '))      adr=tranwrd(adr,' CRSENT '     , ' CRES '     );
            when (index(adr,' RNCHS '))       adr=tranwrd(adr,' RNCHS '      , ' RNCH '     );
            when (index(adr,' CRSCNT '))      adr=tranwrd(adr,' CRSCNT '     , ' CRES '     );
            when (index(adr,' RDGE '))        adr=tranwrd(adr,' RDGE '       , ' RDG '      );
            when (index(adr,' CAUSEWAY '))    adr=tranwrd(adr,' CAUSEWAY '   , ' CSWY '     );
            when (index(adr,' PARKWY '))      adr=tranwrd(adr,' PARKWY '     , ' PKWY '     );
            when (index(adr,' JUNCTON '))     adr=tranwrd(adr,' JUNCTON '    , ' JCT '      );
            when (index(adr,' STATN '))       adr=tranwrd(adr,' STATN '      , ' STA '      );
            when (index(adr,' GARDN '))       adr=tranwrd(adr,' GARDN '      , ' GDN '      );
            when (index(adr,' MNTAIN '))      adr=tranwrd(adr,' MNTAIN '     , ' MTN '      );
            when (index(adr,' CRSSNG '))      adr=tranwrd(adr,' CRSSNG '     , ' XING '     );
            when (index(adr,' RAPID '))       adr=tranwrd(adr,' RAPID '      , ' RPD '      );
            when (index(adr,' KEY '))         adr=tranwrd(adr,' KEY '        , ' KY '       );
            when (index(adr,' WY '))          adr=tranwrd(adr,' WY '         , ' WAY '      );
            when (index(adr,' THROUGHWAY '))  adr=tranwrd(adr,' THROUGHWAY ' , ' TRWY '     );
            when (index(adr,' ESTATES '))     adr=tranwrd(adr,' ESTATES '    , ' ESTS '     );
            when (index(adr,' CK '))          adr=tranwrd(adr,' CK '         , ' CRK '      );
            when (index(adr,' LOAF '))        adr=tranwrd(adr,' LOAF '       , ' LF '       );
            when (index(adr,' HOLLOW '))      adr=tranwrd(adr,' HOLLOW '     , ' HOLW '     );
            when (index(adr,' CANYON '))      adr=tranwrd(adr,' CANYON '     , ' CYN '      );
            when (index(adr,' VILLAGE '))     adr=tranwrd(adr,' VILLAGE '    , ' VLG '      );
            when (index(adr,' CR '))          adr=tranwrd(adr,' CR '         , ' CRK '      );
            when (index(adr,' CT '))          adr=tranwrd(adr,' CT '         , ' CTS '      );
            when (index(adr,' JCTION '))      adr=tranwrd(adr,' JCTION '     , ' JCT '      );
            when (index(adr,' MSSN '))        adr=tranwrd(adr,' MSSN '       , ' MSN '      );
            when (index(adr,' BRDGE '))       adr=tranwrd(adr,' BRDGE '      , ' BRG '      );
            when (index(adr,' CENT '))        adr=tranwrd(adr,' CENT '       , ' CTR '      );
            when (index(adr,' FRT '))         adr=tranwrd(adr,' FRT '        , ' FT '       );
            when (index(adr,' PK '))          adr=tranwrd(adr,' PK '         , ' PARK '     );
            when (index(adr,' LANES '))       adr=tranwrd(adr,' LANES '      , ' LN '       );
            when (index(adr,' GTWAY '))       adr=tranwrd(adr,' GTWAY '      , ' GTWY '     );
            when (index(adr,' PRK '))         adr=tranwrd(adr,' PRK '        , ' PARK '     );
            when (index(adr,' STRAVENUE '))   adr=tranwrd(adr,' STRAVENUE '  , ' STRA '     );
            when (index(adr,' HIWAY '))       adr=tranwrd(adr,' HIWAY '      , ' HWY '      );
            when (index(adr,' VILLE '))       adr=tranwrd(adr,' VILLE '      , ' VL '       );
            when (index(adr,' PLAIN '))       adr=tranwrd(adr,' PLAIN '      , ' PLN '      );
            when (index(adr,' MOUNT '))       adr=tranwrd(adr,' MOUNT '      , ' MT '       );
            when (index(adr,' CENTR '))       adr=tranwrd(adr,' CENTR '      , ' CTR '      );
            when (index(adr,' PRR '))         adr=tranwrd(adr,' PRR '        , ' PR '       );
            when (index(adr,' AVN '))         adr=tranwrd(adr,' AVN '        , ' AVE '      );
            when (index(adr,' SPNG '))        adr=tranwrd(adr,' SPNG '       , ' SPG '      );
            when (index(adr,' HIWY '))        adr=tranwrd(adr,' HIWY '       , ' HWY '      );
            when (index(adr,' DAM '))         adr=tranwrd(adr,' DAM '        , ' DM '       );
            when (index(adr,' CRCL '))        adr=tranwrd(adr,' CRCL '       , ' CIR '      );
            when (index(adr,' SQRE '))        adr=tranwrd(adr,' SQRE '       , ' SQ '       );
            when (index(adr,' JCTN '))        adr=tranwrd(adr,' JCTN '       , ' JCT '      );
            when (index(adr,' MOUNTAIN '))    adr=tranwrd(adr,' MOUNTAIN '   , ' MTN '      );
            when (index(adr,' KEYS '))        adr=tranwrd(adr,' KEYS '       , ' KYS '      );
            when (index(adr,' PARKWAYS '))    adr=tranwrd(adr,' PARKWAYS '   , ' PKWY '     );
            when (index(adr,' DRIVES '))      adr=tranwrd(adr,' DRIVES '     , ' DRS '      );
            when (index(adr,' CENTER '))      adr=tranwrd(adr,' CENTER '     , ' CTR '      );
            when (index(adr,' DRIV '))        adr=tranwrd(adr,' DRIV '       , ' DR '       );
            when (index(adr,' SUMITT '))      adr=tranwrd(adr,' SUMITT '     , ' SMT '      );
            when (index(adr,' CANYN '))       adr=tranwrd(adr,' CANYN '      , ' CYN '      );
            when (index(adr,' HARBR '))       adr=tranwrd(adr,' HARBR '      , ' HBR '      );
            when (index(adr,' REST '))        adr=tranwrd(adr,' REST '       , ' RST '      );
            when (index(adr,' SHOARS '))      adr=tranwrd(adr,' SHOARS '     , ' SHRS '     );
            when (index(adr,' VIST '))        adr=tranwrd(adr,' VIST '       , ' VIS '      );
            when (index(adr,' ISLNDS '))      adr=tranwrd(adr,' ISLNDS '     , ' ISS '      );
            when (index(adr,' HILLS '))       adr=tranwrd(adr,' HILLS '      , ' HLS '      );
            when (index(adr,' CRESENT '))     adr=tranwrd(adr,' CRESENT '    , ' CRES '     );
            when (index(adr,' POINT '))       adr=tranwrd(adr,' POINT '      , ' PT '       );
            when (index(adr,' LAKE '))        adr=tranwrd(adr,' LAKE '       , ' LK '       );
            when (index(adr,' VLLY '))        adr=tranwrd(adr,' VLLY '       , ' VLY '      );
            when (index(adr,' STRAV '))       adr=tranwrd(adr,' STRAV '      , ' STRA '     );
            when (index(adr,' CROSSROAD '))   adr=tranwrd(adr,' CROSSROAD '  , ' XRD '      );
            when (index(adr,' STRAVE '))      adr=tranwrd(adr,' STRAVE '     , ' STRA '     );
            when (index(adr,' STRAVN '))      adr=tranwrd(adr,' STRAVN '     , ' STRA '     );
            when (index(adr,' KNOL '))        adr=tranwrd(adr,' KNOL '       , ' KNL '      );
            when (index(adr,' FORGE '))       adr=tranwrd(adr,' FORGE '      , ' FRG '      );
            when (index(adr,' CNTR '))        adr=tranwrd(adr,' CNTR '       , ' CTR '      );
            when (index(adr,' CAPE '))        adr=tranwrd(adr,' CAPE '       , ' CPE '      );
            when (index(adr,' HEIGHT '))      adr=tranwrd(adr,' HEIGHT '     , ' HTS '      );
            when (index(adr,' HIGHWY '))      adr=tranwrd(adr,' HIGHWY '     , ' HWY '      );
            when (index(adr,' TRNPK '))       adr=tranwrd(adr,' TRNPK '      , ' TPKE '     );
            when (index(adr,' BOULV '))       adr=tranwrd(adr,' BOULV '      , ' BLVD '     );
            when (index(adr,' CIRCLES '))     adr=tranwrd(adr,' CIRCLES '    , ' CIRS '     );
            when (index(adr,' VALLEYS '))     adr=tranwrd(adr,' VALLEYS '    , ' VLYS '     );
            when (index(adr,' VST '))         adr=tranwrd(adr,' VST '        , ' VIS '      );
            when (index(adr,' SPRING '))      adr=tranwrd(adr,' SPRING '     , ' SPG '      );
            when (index(adr,' HOLWS '))       adr=tranwrd(adr,' HOLWS '      , ' HOLW '     );
            when (index(adr,' TRACE '))       adr=tranwrd(adr,' TRACE '      , ' TRCE '     );
            when (index(adr,' BOTTOM '))      adr=tranwrd(adr,' BOTTOM '     , ' BTM '      );
            when (index(adr,' STREME '))      adr=tranwrd(adr,' STREME '     , ' STRM '     );
            when (index(adr,' ISLES '))       adr=tranwrd(adr,' ISLES '      , ' ISLE '     );
            when (index(adr,' CIRC '))        adr=tranwrd(adr,' CIRC '       , ' CIR '      );
            when (index(adr,' FORKS '))       adr=tranwrd(adr,' FORKS '      , ' FRKS '     );
            when (index(adr,' BURG '))        adr=tranwrd(adr,' BURG '       , ' BG '       );
            when (index(adr,' TRLS '))        adr=tranwrd(adr,' TRLS '       , ' TRL '      );
            when (index(adr,' RADIAL '))      adr=tranwrd(adr,' RADIAL '     , ' RADL '     );
            when (index(adr,' LAKES '))       adr=tranwrd(adr,' LAKES '      , ' LKS '      );
            when (index(adr,' EXTENSION '))   adr=tranwrd(adr,' EXTENSION '  , ' EXT '      );
            when (index(adr,' ISLAND '))      adr=tranwrd(adr,' ISLAND '     , ' IS '       );
            when (index(adr,' TERR '))        adr=tranwrd(adr,' TERR '       , ' TER '      );
            when (index(adr,' UNION '))       adr=tranwrd(adr,' UNION '      , ' UN '       );
            when (index(adr,' EXTENSIONS '))  adr=tranwrd(adr,' EXTENSIONS ' , ' EXTS '     );
            when (index(adr,' PKWYS '))       adr=tranwrd(adr,' PKWYS '      , ' PKWY '     );
            when (index(adr,' ISLANDS '))     adr=tranwrd(adr,' ISLANDS '    , ' ISS '      );
            when (index(adr,' ROADS '))       adr=tranwrd(adr,' ROADS '      , ' RDS '      );
            when (index(adr,' GLENS '))       adr=tranwrd(adr,' GLENS '      , ' GLNS '     );
            when (index(adr,' SPRINGS '))     adr=tranwrd(adr,' SPRINGS '    , ' SPGS '     );
            when (index(adr,' MISSN '))       adr=tranwrd(adr,' MISSN '      , ' MSN '      );
            when (index(adr,' RIDGE '))       adr=tranwrd(adr,' RIDGE '      , ' RDG '      );
            when (index(adr,' ARCADE '))      adr=tranwrd(adr,' ARCADE '     , ' ARC '      );
            when (index(adr,' BAYOU '))       adr=tranwrd(adr,' BAYOU '      , ' BYU '      );
            when (index(adr,' CRSNT '))       adr=tranwrd(adr,' CRSNT '      , ' CRES '     );
            when (index(adr,' JUNCTN '))      adr=tranwrd(adr,' JUNCTN '     , ' JCT '      );
            when (index(adr,' FORK '))        adr=tranwrd(adr,' FORK '       , ' FRK '      );
            when (index(adr,' MOUNTAINS '))   adr=tranwrd(adr,' MOUNTAINS '  , ' MTNS '     );
            when (index(adr,' BOTTM '))       adr=tranwrd(adr,' BOTTM '      , ' BTM '      );
            when (index(adr,' FORG '))        adr=tranwrd(adr,' FORG '       , ' FRG '      );
            when (index(adr,' HT '))          adr=tranwrd(adr,' HT '         , ' HTS '      );
            when (index(adr,' FORD '))        adr=tranwrd(adr,' FORD '       , ' FRD '      );
            when (index(adr,' GRDN '))        adr=tranwrd(adr,' GRDN '       , ' GDN '      );
            when (index(adr,' FORT '))        adr=tranwrd(adr,' FORT '       , ' FT '       );
            when (index(adr,' TRACES '))      adr=tranwrd(adr,' TRACES '     , ' TRCE '     );
            when (index(adr,' CNYN '))        adr=tranwrd(adr,' CNYN '       , ' CYN '      );
            when (index(adr,' FLATS '))       adr=tranwrd(adr,' FLATS '      , ' FLTS '     );
            when (index(adr,' ANEX '))        adr=tranwrd(adr,' ANEX '       , ' ANX '      );
            when (index(adr,' GATWAY '))      adr=tranwrd(adr,' GATWAY '     , ' GTWY '     );
            when (index(adr,' RAPIDS '))      adr=tranwrd(adr,' RAPIDS '     , ' RPDS '     );
            when (index(adr,' VILLIAGE '))    adr=tranwrd(adr,' VILLIAGE '   , ' VLG '      );
            when (index(adr,' COVES '))       adr=tranwrd(adr,' COVES '      , ' CVS '      );
            when (index(adr,' RVR '))         adr=tranwrd(adr,' RVR '        , ' RIV '      );
            when (index(adr,' AV '))          adr=tranwrd(adr,' AV '         , ' AVE '      );
            when (index(adr,' PIKES '))       adr=tranwrd(adr,' PIKES '      , ' PIKE '     );
            when (index(adr,' VISTA '))       adr=tranwrd(adr,' VISTA '      , ' VIS '      );
            when (index(adr,' FORESTS '))     adr=tranwrd(adr,' FORESTS '    , ' FRST '     );
            when (index(adr,' FIELD '))       adr=tranwrd(adr,' FIELD '      , ' FLD '      );
            when (index(adr,' BRANCH '))      adr=tranwrd(adr,' BRANCH '     , ' BR '       );
            when (index(adr,' DALE '))        adr=tranwrd(adr,' DALE '       , ' DL '       );
            when (index(adr,' ANNEX '))       adr=tranwrd(adr,' ANNEX '      , ' ANX '      );
            when (index(adr,' SQR '))         adr=tranwrd(adr,' SQR '        , ' SQ '       );
            when (index(adr,' COVE '))        adr=tranwrd(adr,' COVE '       , ' CV '       );
            when (index(adr,' SQU '))         adr=tranwrd(adr,' SQU '        , ' SQ '       );
            when (index(adr,' SKYWAY '))      adr=tranwrd(adr,' SKYWAY '     , ' SKWY '     );
            when (index(adr,' RIDGES '))      adr=tranwrd(adr,' RIDGES '     , ' RDGS '     );
            when (index(adr,' TUNNL '))       adr=tranwrd(adr,' TUNNL '      , ' TUNL '     );
            when (index(adr,' UNDERPASS '))   adr=tranwrd(adr,' UNDERPASS '  , ' UPAS '     );
            when (index(adr,' CLIFF '))       adr=tranwrd(adr,' CLIFF '      , ' CLF '      );
            when (index(adr,' LANE '))        adr=tranwrd(adr,' LANE '       , ' LN '       );
            when (index(adr,' DVD '))         adr=tranwrd(adr,' DVD '        , ' DV '       );
            when (index(adr,' CURVE '))       adr=tranwrd(adr,' CURVE '      , ' CURV '     );
            when (index(adr,' SUMMIT '))      adr=tranwrd(adr,' SUMMIT '     , ' SMT '      );
            when (index(adr,' GARDENS '))     adr=tranwrd(adr,' GARDENS '    , ' GDNS '     );
            otherwise;
         end;
    ;;;;
    run;quit;

    /*---- from my autoexec                                                  ----*/

    %let states50q="AL","AK","AZ","AR","CA","CO","CT","DE","FL","GA","HI","ID","IL","IN","IA","KS","KY","LA","ME","MD","MA","MI","MN","MS","MO","MT",
    "NE","NV","NH","NJ","NM","NY","NC","ND","OH","OK","OR","PA","RI","SC","SD","TN","TX","UT","VT","VA","WA","WV","WI","WY";

    %stop_submission;

    libname adr "d:/adr/sd1/fin";

    /*   _            _            __   _
      __| | ___ _ __ | |_    ___  / _| | |_ _ __ __ _ _ __  ___
     / _` |/ _ \ `_ \| __|  / _ \| |_  | __| `__/ _` | `_ \/ __|
    | (_| |  __/ |_) | |_  | (_) |  _| | |_| | | (_| | | | \__ \
     \__,_|\___| .__/ \__|  \___/|_|    \__|_|  \__,_|_| |_|___/
               |_|
    */

    /*---- https://tinyurl.com/327p452e                                      ----*/

    data nad.nad_20231101 (compress=binary );;

      label OID="Col1";
      label AddNum_Pre="Col2";
      label Add_Number="Col3";
      label AddNum_Suf="Col4";
      label AddNo_Full="Col5";
      label St_PreMod="Col6";
      label St_PreDir="Col7";
      label St_PreTyp="Col8";
      label St_PreSep="Col9";
      label St_Name="Col10";
      label St_PosTyp="Col11";
      label St_PosDir="Col12";
      label St_PosMod="Col13";
      label StNam_Full="Col14";
      label Building="Col15";
      label Floor="Col16";
      label Unit="Col17";
      label Room="Col18";
      label Seat="Col19";
      label Addtl_Loc="Col20";
      label SubAddress="Col21";
      label LandmkName="Col22";
      label County="Col23";
      label Inc_Muni="Col24";
      label Post_City="Col25";
      label Census_Plc="Col26";
      label Uninc_Comm="Col27";
      label Nbrhd_Comm="Col28";
      label NatAmArea="Col29";
      label NatAmSub="Col30";
      label Urbnztn_PR="Col31";
      label PlaceOther="Col32";
      label PlaceNmTyp="Col33";
      label State="Col34";
      label Zip_Code="Col35";
      label Plus_4="Col36";
      label UUID="Col37";
      label AddAuth="Col38";
      label AddrRefSys="Col39";
      label Longitude="Col40";
      label Latitude="Col41";
      label NatGrid="Col42";
      label Elevation="Col43";
      label Placement="Col44";
      label AddrPoint="Col45";
      label Related_ID="Col46";
      label RelateType="Col47";
      label ParcelSrc="Col48";
      label Parcel_ID="Col49";
      label AddrClass="Col50";
      label Lifecycle="Col51";
      label Effective_Date="Col52";
      label Expire_Date="Col53";
      label DateUpdate_Date="Col54";
      label AnomStatus="Col55";
      label LocatnDesc="Col56";
      label Addr_Type="Col57";
      label DeliverTyp="Col58";
      label NAD_Source="Col59";
      label DataSet_ID="Col60";


      informat AddNum_Pre $15.;
      informat AddNum_Suf $15.;
      informat AddNo_Full $100.;
      informat St_PreMod $15.;
      informat St_PreDir $10.;
      informat St_PreTyp $50.;
      informat St_PreSep $20.;
      informat St_Name $254.;
      informat St_PosTyp $50.;
      informat St_PosDir $10.;
      informat St_PosMod $25.;
      informat StNam_Full $255.;
      informat Building $75.;
      informat Floor $75.;
      informat Unit $75.;
      informat Room $75.;
      informat Seat $75.;
      informat Addtl_Loc $225.;
      informat SubAddress $255.;
      informat LandmkName $150.;
      informat County $100.;
      informat Inc_Muni $100.;
      informat Post_City $40.;
      informat Census_Plc $100.;
      informat Uninc_Comm $100.;
      informat Nbrhd_Comm $100.;
      informat NatAmArea $100.;
      informat NatAmSub $100.;
      informat Urbnztn_PR $100.;
      informat PlaceOther $100.;
      informat PlaceNmTyp $50.;
      informat State $2.;
      informat Zip_Code $7.;
      informat Plus_4 $4.;
      informat UUID $32.;
      informat AddAuth $100.;
      informat AddrRefSys $75.;
      informat NatGrid $50.;
      informat Placement $25.;
      informat AddrPoint $50.;
      informat Related_ID $50.;
      informat RelateType $50.;
      informat ParcelSrc $50.;
      informat Parcel_ID $50.;
      informat AddrClass $50.;
      informat Lifecycle $50.;
      informat Effective_Date $17.;
      informat Expire_Date $17.;
      informat DateUpdate_Date $17.;
      informat AnomStatus $50.;
      informat LocatnDesc $75.;
      informat Addr_Type $50.;
      informat DeliverTyp $50.;
      informat NAD_Source $75.;
      informat DataSet_ID $254.;

      infile "d:/adr/NAD20231101.txt" lrecl=32756 recfm=v MISSOVER DSD firstobs=2;

      INPUT
      OID
      AddNum_Pre
      Add_Number
      AddNum_Suf
      AddNo_Full
      St_PreMod
      St_PreDir
      St_PreTyp
      St_PreSep
      St_Name
      St_PosTyp
      St_PosDir
      St_PosMod
      StNam_Full
      Building
      Floor
      Unit
      Room
      Seat
      Addtl_Loc
      SubAddress
      LandmkName
      County
      Inc_Muni
      Post_City
      Census_Plc
      Uninc_Comm
      Nbrhd_Comm
      NatAmArea
      NatAmSub
      Urbnztn_PR
      PlaceOther
      PlaceNmTyp
      State
      Zip_Code
      Plus_4
      UUID
      AddAuth
      AddrRefSys
      Longitude
      Latitude
      NatGrid
      Elevation
      Placement
      AddrPoint
      Related_ID
      RelateType
      ParcelSrc
      Parcel_ID
      AddrClass
      Lifecycle
      Effective_Date
      Expire_Date
      DateUpdate_Date
      AnomStatus
      LocatnDesc
      Addr_Type
      DeliverTyp
      NAD_Source
      DataSet_ID
      ;

      if mod(_n_,1000000)=0 then put _n_ comma18.;

      run;quit;
    ');

    Middle Observation(38417884 ) of table = nad.nad_20231101 - Total Obs 0 23MAY2022:11:30:21


     -- CHARACTER --
    Variable                        Typ    Value                      Label

    ADDNUM_PRE                       C15                              Col2
    ADDNUM_SUF                       C15                              Col4
    ADDNO_FULL                       C100  303                        Col5
    ST_PREMOD                        C15                              Col6
    ST_PREDIR                        C10   East                       Col7
    ST_PRETYP                        C50                              Col8
    ST_PRESEP                        C20                              Col9
    ST_NAME                          C254  14TH                       Col10
    ST_POSTYP                        C50   Street                     Col11
    ST_POSDIR                        C10                              Col12
    ST_POSMOD                        C25                              Col13
    STNAM_FULL                       C255  East 14TH Street           Col14
    BUILDING                         C75                              Col15
    FLOOR                            C75                              Col16
    UNIT                             C75                              Col17
    ROOM                             C75                              Col18
    SEAT                             C75                              Col19
    ADDTL_LOC                        C225                             Col20
    SUBADDRESS                       C255                             Col21
    LANDMKNAME                       C150                             Col22
    COUNTY                           C100  Richardson                 Col23
    INC_MUNI                         C100  FALLS CITY                 Col24
    POST_CITY                        C40   FALLS CITY                 Col25
    CENSUS_PLC                       C100                             Col26
    UNINC_COMM                       C100                             Col27
    NBRHD_COMM                       C100                             Col28
    NATAMAREA                        C100                             Col29
    NATAMSUB                         C100                             Col30
    URBNZTN_PR                       C100                             Col31
    PLACEOTHER                       C100                             Col32
    PLACENMTYP                       C50                              Col33
    STATE                            C2    NE                         Col34
    ZIP_CODE                         C7    68355                      Col35
    PLUS_4                           C4                               Col36
    UUID                             C32   {6C7E0370-3C9F-4741-81DF-F6Col37
    ADDAUTH                          C100  SOUTHEAST                  Col38
    ADDRREFSYS                       C75                              Col39
    NATGRID                          C50   15TTE7821837495            Col42
    PLACEMENT                        C25   Unknown                    Col44
    ADDRPOINT                        C50   -95.60027125699997 40.05854Col45
    RELATED_ID                       C50                              Col46
    RELATETYPE                       C50                              Col47
    PARCELSRC                        C50                              Col48
    PARCEL_ID                        C50                              Col49
    ADDRCLASS                        C50                              Col50
    LIFECYCLE                        C50                              Col51
    EFFECTIVE_DATE                   C17                              Col52
    EXPIRE_DATE                      C17                              Col53
    DATEUPDATE_DATE                  C17   9/26/2019 14:35:2          Col54
    ANOMSTATUS                       C50                              Col55
    LOCATNDESC                       C75                              Col56
    ADDR_TYPE                        C50   Unknown                    Col57
    DELIVERTYP                       C50                              Col58
    NAD_SOURCE                       C75   State of Nebraska          Col59
    DATASET_ID                       C254  SSAP_70529@richardson.ne.usCol60
    TOTOBS                           C16   0                          TOTOBS


     -- NUMERIC --
    OID                              N8              -1               Col1
    ADD_NUMBER                       N8             303               Col3
    LONGITUDE                        N8    -95.60027126               Col40
    LATITUDE                         N8    40.058549683               Col41
    ELEVATION                        N8               .               Col43



    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  Middle Observation(38417884 ) of table = nad.nad_20231101                                                             */
    /*                                                                                                                        */
    /*   -- CHARACTER --                                                                                                      */
    /*  Variable                        Typ    Value                       Label                                              */
    /*                                                                                                                        */
    /*  ADDNUM_PRE                       C15                               Col2                                               */
    /*  ADDNUM_SUF                       C15                               Col4                                               */
    /*  ADDNO_FULL                       C100  303                         Col5                                               */
    /*  ST_PREMOD                        C15                               Col6                                               */
    /*  ST_PREDIR                        C10   East                        Col7                                               */
    /*  ST_PRETYP                        C50                               Col8                                               */
    /*  ST_PRESEP                        C20                               Col9                                               */
    /*  ST_NAME                          C254  14TH                        Col10                                              */
    /*  ST_POSTYP                        C50   Street                      Col11                                              */
    /*  ST_POSDIR                        C10                               Col12                                              */
    /*  ST_POSMOD                        C25                               Col13                                              */
    /*  STNAM_FULL                       C255  East 14TH Street            Col14                                              */
    /*  BUILDING                         C75                               Col15                                              */
    /*  FLOOR                            C75                               Col16                                              */
    /*  UNIT                             C75                               Col17                                              */
    /*  ROOM                             C75                               Col18                                              */
    /*  SEAT                             C75                               Col19                                              */
    /*  ADDTL_LOC                        C225                              Col20                                              */
    /*  SUBADDRESS                       C255                              Col21                                              */
    /*  LANDMKNAME                       C150                              Col22                                              */
    /*  COUNTY                           C100  Richardson                  Col23                                              */
    /*  INC_MUNI                         C100  FALLS CITY                  Col24                                              */
    /*  POST_CITY                        C40   FALLS CITY                  Col25                                              */
    /*  CENSUS_PLC                       C100                              Col26                                              */
    /*  UNINC_COMM                       C100                              Col27                                              */
    /*  NBRHD_COMM                       C100                              Col28                                              */
    /*  NATAMAREA                        C100                              Col29                                              */
    /*  NATAMSUB                         C100                              Col30                                              */
    /*  URBNZTN_PR                       C100                              Col31                                              */
    /*  PLACEOTHER                       C100                              Col32                                              */
    /*  PLACENMTYP                       C50                               Col33                                              */
    /*  STATE                            C2    NE                          Col34                                              */
    /*  ZIP_CODE                         C7    68355                       Col35                                              */
    /*  PLUS_4                           C4                                Col36                                              */
    /*  UUID                             C32   {6C7E0370-3C9F-4741-81DF-F6 Col37                                              */
    /*  ADDAUTH                          C100  SOUTHEAST                   Col38                                              */
    /*  ADDRREFSYS                       C75                               Col39                                              */
    /*  NATGRID                          C50   15TTE7821837495             Col42                                              */
    /*  PLACEMENT                        C25   Unknown                     Col44                                              */
    /*  ADDRPOINT                        C50   -95.60027125699997 40.05854 Col45                                              */
    /*  RELATED_ID                       C50                               Col46                                              */
    /*  RELATETYPE                       C50                               Col47                                              */
    /*  PARCELSRC                        C50                               Col48                                              */
    /*  PARCEL_ID                        C50                               Col49                                              */
    /*  ADDRCLASS                        C50                               Col50                                              */
    /*  LIFECYCLE                        C50                               Col51                                              */
    /*  EFFECTIVE_DATE                   C17                               Col52                                              */
    /*  EXPIRE_DATE                      C17                               Col53                                              */
    /*  DATEUPDATE_DATE                  C17   9/26/2019 14:35:2           Col54                                              */
    /*  ANOMSTATUS                       C50                               Col55                                              */
    /*  LOCATNDESC                       C75                               Col56                                              */
    /*  ADDR_TYPE                        C50   Unknown                     Col57                                              */
    /*  DELIVERTYP                       C50                               Col58                                              */
    /*  NAD_SOURCE                       C75   State of Nebraska           Col59                                              */
    /*  DATASET_ID                       C254  SSAP_70529@richardson.ne.us Col60                                              */
    /*  TOTOBS                           C16   0                           TOTOBS                                             */
    /*                                                                                                                        */
    /*                                                                                                                        */
    /*   -- NUMERIC --                                                                                                        */
    /*  OID                              N8              -1                Col1                                               */
    /*  ADD_NUMBER                       N8             303                Col3                                               */
    /*  LONGITUDE                        N8    -95.60027126                Col40                                              */
    /*  LATITUDE                         N8    40.058549683                Col41                                              */
    /*  ELEVATION                        N8               .                Col43                                              */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    data adr_010nadfix (compress=binary);

      keep
         ADR
         NUMBER
         ZIPCODE
         LON
         LAT
         STATE
      ;


      length
        ADR          $100
        NUMBER       $100
        ZIPCODE        $7
        STATE          $2
       ;

      set nad.nad_20231101 (keep=
               addno_full
               stnam_full
               state
               addrpoint
               zip_code
               ) ;

        if mod(_n_,1000000)=0 then put _n_ comma18.;

        *if state = "CA" then stop;

        lon=input(scan(addrpoint,1,' '),best24.17);
        lat=input(scan(addrpoint,2,' '),best24.17);

        number    = addno_full;
        zipcode   = zip_code;

        adr=compress(upcase(catx(' ',compress(number),stnam_full)),'.','kads');

        if countw(adr) > 2 then do;
           len=length(strip(scan(adr,-1,' ')));
           sfx=strip(scan(adr,-1,' '));
           select;
               %include "%sysfunc(pathname(work))/adr_010sfx.sas";
              otherwise;
           end;
        end;

        %inc "%sysfunc(pathname(work))/adr_010sx1.sas" ;

        adr=tranwrd(adr,'NORTH','N');
        adr=tranwrd(adr,'SOUTH','S');
        adr=tranwrd(adr,'EAST','E');
        adr=tranwrd(adr,'WEST','W');
        adr=tranwrd(adr,'NORTHEAST','NE');
        adr=tranwrd(adr,'SOUTHEAST','SE');
        adr=tranwrd(adr,'NORTHWEST','NW');
        adr=tranwrd(adr,'SOUTHWEST','SW');

    run;quit;

    proc sql;
      create
          index state
       on
          adr_010nadfix
    ;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  p to 40 obs from last table WORK.ADR_010NADFIX total obs=237,889 23MAY2022:10:33:31                                   */
    /*                                                                                                                        */
    /*    Obs    ADR                      NUMBER    ZIPCODE    STATE       LON        LAT                                     */
    /*                                                                                                                        */
    /*      1    26800 EKLUTNA VLG RD     26800      99567      AK      -149.376    61.4655                                   */
    /*      2    26960 ADAK CIR           26960      99577      AK      -149.360    61.4636                                   */
    /*      3    26636 EKLUTNA VLG RD     26636      99567      AK      -149.361    61.4607                                   */
    /*      4    26585 EKLUTNA VLG RD     26585      99567      AK      -149.361    61.4606                                   */
    /*      5    26612 EKLUTNA VLG RD     26612      99567      AK      -149.362    61.4604                                   */
    /*      6    26527 EKLUTNA VLG RD     26527      99567      AK      -149.361    61.4602                                   */
    /*      7    26450 EKLUTNA VLG RD     26450      99567      AK      -149.362    61.4600                                   */
    /*      8    26428 EKLUTNA VLG RD     26428      99567      AK      -149.363    61.4597                                   */
    /*      9    26439 EKLUTNA VLG RD     26439      99567      AK      -149.361    61.4597                                   */
    /*     10    28350 GLACIER LOOP RD    28350      99567      AK      -149.355    61.4592                                   */
    /*     11    26339 EKLUTNA VLG RD     26339      99567      AK      -149.362    61.4584                                   */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*               _
     _ __   __ _  __| |
    | `_ \ / _` |/ _` |
    | | | | (_| | (_| |
    |_| |_|\__,_|\__,_|

    */

    %macro stsnad(_st);

          %local obs;
          /* %let _st=AK;  */


           proc datasets lib=sd1 nolist nodetails;delete adr_010&_st.nadall; run;quit;
           proc datasets lib=work nolist nodetails;delete &_st.notzip &_st.haszip adr_010&_st.nadpre; run;quit;

           /*         _ _ _         _                        _
            ___ _ __ | (_) |_   ___(_)_ __    _ __   ___ ___(_)_ __
           / __| `_ \| | | __| |_  / | `_ \  | `_ \ / _ \_  / | `_ \
           \__ \ |_) | | | |_   / /| | |_) | | | | | (_) / /| | |_) |
           |___/ .__/|_|_|\__| /___|_| .__/  |_| |_|\___/___|_| .__/
               |_|                   |_|                      |_|
           */

           data &_st.notzip  &_st.haszip;

             format lat lon 23.16;

             set adr_010nadfix (where=(state="&_st"));

             zipcode=compress(substr(put(input(substr(left(zipcode),1,5)!!'?????',?? 5.),?? z5.),1,5),'.');

             if length(strip(compress(zipcode)))<5 or missing(input(substr(left(zipcode),1,5),?? 5.)) then output &_st.notzip;
             else output &_st.haszip;


           run;quit;

           /**************************************************************************************************************************/
           /*  _                     _                                                                                               */
           /* | |__   __ _ ___   ___(_)_ __                                                                                          */
           /* | `_ \ / _` / __| |_  / | `_ \                                                                                         */
           /* | | | | (_| \__ \  / /| | |_) |                                                                                        */
           /* |_| |_|\__,_|___/ /___|_| .__/                                                                                         */
           /*                         |_|                                                                                            */
           /*                                                                                                                        */
           /* WORK.AKHASZIP total obs=170,473                                                                                        */
           /*                                                                                                                        */
           /*   Obs      LAT         LON      ADR                      NUMBER    ZIPCODE    STATE                                    */
           /*                                                                                                                        */
           /*     1    61.4655    -149.376    26800 EKLUTNA VLG RD     26800      99567      AK                                      */
           /*     2    61.4636    -149.360    26960 ADAK CIR           26960      99577      AK                                      */
           /*     3    61.4607    -149.361    26636 EKLUTNA VLG RD     26636      99567      AK                                      */
           /*     4    61.4606    -149.361    26585 EKLUTNA VLG RD     26585      99567      AK                                      */
           /*     5    61.4604    -149.362    26612 EKLUTNA VLG RD     26612      99567      AK                                      */
           /*     6    61.4602    -149.361    26527 EKLUTNA VLG RD     26527      99567      AK                                      */
           /*     7    61.4600    -149.362    26450 EKLUTNA VLG RD     26450      99567      AK                                      */
           /*     8    61.4597    -149.363    26428 EKLUTNA VLG RD     26428      99567      AK                                      */
           /*                                                                                                                        */
           /*                    _                                                                                                   */
           /*  _ __   ___    ___(_)_ __                                                                                              */
           /* | `_ \ / _ \  |_  / | `_ \                                                                                             */
           /* | | | | (_) |  / /| | |_) |                                                                                            */
           /* |_| |_|\___/  /___|_| .__/                                                                                             */
           /*                     |_|                                                                                                */
           /*                                                                                                                        */
           /* Up to 40 obs from AKNOTZIP total obs=67,416 23MAY2022:10:36:17                                                         */
           /*   Obs      LAT         LON      ADR                       NUMBER    ZIPCODE    STATE                                   */
           /*                                                                                                                        */
           /*     1    59.0833    -158.582    4928 ALDER ST              4928                 AK                                     */
           /*     2    59.0815    -158.578    4804 ASPEN ST              4804                 AK                                     */
           /*     3    59.0827    -158.585    5027 ALDER ST              5027                 AK                                     */
           /*     4    59.0814    -158.585    5024 ASPEN ST              5024                 AK                                     */
           /*     5    59.0815    -158.587    5052 ASPEN ST              5052                 AK                                     */
           /*     6    59.0804    -158.577    4145 SPRUCE ST             4145                 AK                                     */
           /*     7    59.0807    -158.575    4142 SPRUCE ST             4142                 AK                                     */
           /*     8    59.0797    -158.588    4101 ALEKNAGIK LK RD       4101                 AK                                     */
           /*     9    59.0387    -158.460    111 CENTRAL AVE            111                  AK                                     */
           /*    10    59.0385    -158.460    103 CENTRAL AVE            103                  AK                                     */
           /*                                                                                                                        */
           /**************************************************************************************************************************/

           proc sql;
              select count(*) into :zro from &_st.notzip
           ;quit;

           %if &zro = 0 %then %do;
              data adr_010&_st.nadall1st;
                set  &_st.haszip;
                gepdist=99999;
                adr=catx(' ',adr,zipcode);
                drop number;
              run;quit;
              %goto sumup;
           %end;

           proc sort data=&_st.notzip out=&_st.notzipunq;
             by lat lon adr;
           run;quit;


           data adr_010quad;
              set adr_010UsPlaceZip(where=(state="&_st") rename=(postalcode=zipcode admin_code1=state));
           run;quit;

           /**************************************************************************************************************************/
           /*                                                                                                                        */
           /* AR_010QUAD OBS=273                                                                                                     */
           /*                                                                                                                        */
           /*  Obs    LONGITUDE    LATITUDE    ZIPCODE    STATE                                                                      */
           /*                                                                                                                        */
           /*    1     -149.867     61.2187     99501      AK                                                                        */
           /*    2     -149.943     61.1491     99502      AK                                                                        */
           /*    3     -149.888     61.1934     99503      AK                                                                        */
           /*    4     -149.748     61.2045     99504      AK                                                                        */
           /*    5     -149.689     61.2560     99505      AK                                                                        */
           /*    6     -149.789     61.2321     99506      AK                                                                        */
           /*    7     -149.828     61.1520     99507      AK                                                                        */
           /*    8     -149.816     61.2013     99508      AK                                                                        */
           /*    9     -149.878     61.1756     99509      AK                                                                        */
           /*                                                                                                                        */
           /**************************************************************************************************************************/

           %macro slicen(first=);

                /*   %let first=1; */

                 %let obs=%eval(&first + 9999);

               proc sql;
                 create
                    table adr_010&_st.nad&first (compress=binary) as
                 select
                  distinct
                    l.state
                   ,l.adr
                   ,l.lon
                   ,l.lat
                   ,r.zipcode
                   ,geodist(l.lat,l.lon,r.latitude,r.longitude,'M') as gepdist format=23.16
                 from
                   &_st.notzip(firstobs=&first obs=&obs ) as l left join
                     adr_010quad as r
                 on
                   1=1
                 group
                     by l.lon, l.lat
                 having
                     geodist(l.lat,l.lon,r.latitude,r.longitude,'M')
                       = min(geodist(l.lat,l.lon,r.latitude,r.longitude,'M'))
                 order
                     by adr, zipcode, lon, lat
               ;quit;

               proc append base=adr_010&_st.nadpre  data=adr_010&_st.nad&first;
               run;quit;

           %mend slicen;

           /*                        _              _             _  ___  _
             _____  _____  ___ _   _| |_ ___ _ __  | |__  _   _  / |/ _ \| | __
            / _ \ \/ / _ \/ __| | | | __/ _ \ `__| | `_ \| | | | | | | | | |/ /
           |  __/>  <  __/ (__| |_| | ||  __/ |    | |_) | |_| | | | |_| |   <
            \___/_/\_\___|\___|\__,_|\__\___|_|    |_.__/ \__, | |_|\___/|_|\_\
                                                          |___/
           */

           data _null_;
             retain obs 9999 cnt10k 0 cnt 0;
             set &_st.notzipunq;
             n=_n_-1;
             if mod(n,10000)=0 then do;
                cnt=cnt+1;
                cnt10k =cnt*20000 - 19999;;
                putlog n cnt cnt10k;
                output;
                call execute(cats('%slicen(first=',cnt10k,');'));
             end;
           run;quit;

            /*
              _ _             _                                     __ _          _        _       _
            / _(_)_ __   __ _| |  _ __ ___   ___  __ _ _ __  ___   / _(_)_ __ ___| |_   __| | ___ | |_
           | |_| | `_ \ / _` | | | `_ ` _ \ / _ \/ _` | `_ \/ __| | |_| | `__/ __| __| / _` |/ _ \| __|
           |  _| | | | | (_| | | | | | | | |  __/ (_| | | | \__ \ |  _| | |  \__ \ |_ | (_| | (_) | |_
           |_| |_|_| |_|\__,_|_| |_| |_| |_|\___|\__,_|_| |_|___/ |_| |_|_|  |___/\__| \__,_|\___/ \__|

           */

           data adr_010&_st.nadall1st;
             set
                adr_010&_st.nadpre
                &_st.haszip
             ;

             lon=round(lon,.00001);
             lat=round(lat,.00001);

             adr=catx(' ',adr,zipcode);

             if missing(gepdist) then gepdist=99999;

             drop number;
           run;quit;

           %sumup:

           proc sort data=adr_010&_st.nadall1st out=adr_010&_st.nadall2nd ;
            by adr lon lat state gepdist;
           run;quit;

           data adr_010&_st.nadall3rd;

              length zip4 $4.;

              set adr_010&_st.nadall2nd;
              by adr lon lat state;

              if first.state;
              zip4=substr(zipcode,1,4);

           run;quit;

           proc summary data=adr_010&_st.nadall3rd nway;
             by adr zip4 state;
             var lon lat;
           output out=adr_010&_st.nadall(compress=char drop=_type_ _freq_) mean=avglat avglon;
           run;quit;

           proc append data=adr_010&_st.nadall base= nad.adr_010nadall;
           run;quit;

    %mend stsnad;

    %symdel st / nowarn;

    proc datasets lib=nad nolist nodetails;delete adr_010nadall; run;quit;

    %stsnad(AK);
    %stsnad(AL);
    %stsnad(AR);
    %stsnad(AZ);
    %stsnad(CA);
    %stsnad(CO);
    %stsnad(CT);
    %stsnad(DC);
    %stsnad(DE);
    %*stsnad(FL);  /* not present */
    %*stsnad(GA);  /* not present */
    %*stsnad(HI);  /* not present */
    %stsnad(IA);
    %*stsnad(ID);   /* not present */
    %stsnad(IL);
    %stsnad(IN);
    %stsnad(KS);
    %stsnad(KY);
    %stsnad(LA);
    %stsnad(MA);
    %stsnad(MD);
    %stsnad(ME);
    %*stsnad(MI);  /* not present */
    %stsnad(MN);
    %stsnad(MO);
    %*stsnad(MS);  /* not present */
    %stsnad(MT);
    %stsnad(NC);
    %stsnad(ND);
    %stsnad(NE);
    %*stsnad(NH);  /* not present */
    %stsnad(NJ);
    %stsnad(NM);
    %*stsnad(NV); /* not present */
    %stsnad(NY);
    %stsnad(OH);
    %stsnad(OK);
    %*stsnad(OR);  /* not present */
    %stsnad(PA);
    %stsnad(RI);
    %stsnad(SC);
    %stsnad(SD);
    %stsnad(TN);
    %stsnad(UT);
    %stsnad(VA);
    %stsnad(VT);
    %stsnad(WA);
    %stsnad(WI);
    %*stsnad(WV); /* not present */
    %stsnad(WY);
    %stsnad(TX);


    proc sql;
      select count(*) into :_obs from nad.adr_010nadall
    ;quit;

    Data stnad (compress=char);
      retain zro 0;
      set nad.adr_010nadall ( keep=adr zip4
             where=((uniform(13976)*&_obs)<5000000));
    run;quit;


    %*inc "c:/oto/oto_voodoo.sas";

    %utlvdoc
        (
        libname        = work           /* libname of input dataset */
        ,data          = stnad /* name of input dataset */
        ,key           = 0  /* 0 or variable */
        ,ExtrmVal      = 1000           /* display top and bottom 30 frequencies */
        ,UniPlot       = 0            /* 'true' enables ('false' disables) plot option on univariate output
        ,UniVar        = 0            /* 'true' enables ('false' disables) plot option on univariate output
        ,misspat       = 0            /* 0 or 1 missing patterns */
        ,chart         = 0            /* 0 or 1 line printer chart */
        ,taball        = state zipstate            /* all pairs */
        ,tabone        = 0     /* 0 or  variable vs all other variables          */
        ,mispop        = 0            /* 0 or 1  missing vs populated*/
        ,mispoptbl     = 0            /* 0 or 1  missing vs populated*/
        ,dupcol        = 0            /* 0 or 1  columns duplicated  */
        ,unqtwo        = 0            /* 0 */
        ,vdocor        = 0            /* 0 or 1  correlation of numeric variables */
        ,oneone        = 0            /* 0 or 1  one to one - one to many - many to many */
        ,cramer        = 0            /* 0 or 1  association of character variables    */
        ,optlength     = 0
        ,maxmin        = 0
        ,unichr        = 0
        ,outlier       = 0
        ,printto       = d:\adr\vdo\&data..txt        /* file or output if output window */
        ,Cleanup       = 0           /* 0 or 1 delete intermediate datasets */
        );

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
