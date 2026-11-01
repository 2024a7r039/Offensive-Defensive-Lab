# Experiment 7: Passive Reconnaissance of Rockstar Games

## Aim
To gather publicly available information about Rockstar Games using Google search operators and Shodan.

## Methodology
Google was used with the `site:rockstargames.com`, `intitle:"Rockstar Games"`, and `filetype:pdf` search operators. Shodan was viewed first at its homepage, then searched with `hostname:rockstargames.com`. Only publicly indexed information was observed; no hosts were accessed or scanned.

## Results
Google returned Rockstar-related pages and PDF results. The Shodan hostname search displayed publicly indexed Rockstar-related hosts and service information.

## Screenshots

### 1. Google `site:rockstargames.com`
![Google site search](Screenshot%202026-11-01%20202811(1).png)

### 2. Google `intitle:"Rockstar Games"`
![Google intitle search](Screenshot%202026-11-01%20202833(1).png)

### 3. Google `filetype:pdf`
![Google PDF search](Screenshot%202026-11-01%20202850(1).png)

### 4. Shodan homepage
![Shodan homepage](Screenshot%202026-11-01%20202947.png)

### 5. Shodan `hostname:rockstargames.com`
![Shodan hostname search](Screenshot%202026-11-01%20203218.png)
