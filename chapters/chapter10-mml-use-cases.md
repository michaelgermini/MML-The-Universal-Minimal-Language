# Chapter 10 — MML Use Cases

## 10.1 War zones

### Communication under conflict

#### Specific challenges
- **Destroyed infrastructures**: Cut electrical networks
- **Radio jamming**: Military communications
- **Constant movement**: Mobile troops
- **Extreme stress**: Personnel under pressure

#### MML solutions
- **Morse transmission**: Bypass jamming
- **Human messengers**: Couriers on foot
- **Fragmented DNF**: Resist interruptions

#### Operational example
```
T:COMBAT REPORT
M:SECTOR|ALPHA-7
M:DATE|15NOV2025
H:SITUATION
P:ENEMY REPELLED POSITION 45.2N 2.1E
M:LOSSES|3 WOUNDED 1 KILLED
H:REINFORCEMENTS
P:URGENT NEED DOCTORS
L:TACTICAL MAP|map-alpha7.png
Q:PRIORITY AIR EXTRACTION
```

## 10.2 Natural disasters

### Post-disaster coordination

#### Typical scenarios
- **Earthquakes**: Collapsed infrastructures
- **Floods**: Isolated areas
- **Hurricanes**: Cut communications
- **Fires**: Massive evacuation

#### MML applications
- **Resource inventories**: Water, food, medications
- **Rescue coordination**: Team positions
- **Damage assessment**: Structured reports

#### Damage assessment report
```
T:DAMAGE ASSESSMENT
H:DISASTER ZONE
M:AREA|5km²
M:BUILDINGS|80% DESTROYED
M:VICTIMS|12 MISSING
P:ELECTRICITY CUT NORTH SECTOR
P:ACCESS ROADS BLOCKED
H:REQUIRED RESOURCES
P:RESCUE TEAM 20 PEOPLE
P:HEAVY EQUIPMENT PRIORITY
M:COORD|43.5N 1.4W
```

## 10.3 Offline communication

### Autonomous systems

#### Community library
- **Cataloging**: Available books
- **Reservations**: System without electricity
- **Exchanges**: Local community

#### Rental management
```
T:RENTAL STATUS
H:BUILDING A
M:APARTMENTS|24
M:OCCUPIED|18
M:AVAILABLE|6
P:REPAIRS NEEDED STAIRWAY B
H:BUILDING B
M:APARTMENTS|16
M:OCCUPIED|16
M:AVAILABLE|0
```

## 10.4 Long distance low tech

### Isolated expeditions

#### Polar research
- **Scientific data**: Automatic collection
- **Health reports**: Isolated team
- **Maintenance**: Equipment monitoring

#### Iridium satellite transmission
```
PKT:EXPEDITION-045
T:DAILY REPORT
M:DAY|45
M:POSITION|78.2S 45.1E
H:WEATHER
M:TEMPERATURE|-35°C
M:WIND|45km/h
H:TEAM
P:HEALTH GOOD NO PROBLEMS
P:NORMAL PROGRESSION
H:DATA
M:SAMPLES|12
M:OBSERVATIONS|8
END
```

## 10.5 Amateur radio

### Emergency communication

#### ARES/RACES networks
- **Frequency management**: Coordination
- **Alert broadcasting**: Civilian population
- **Logistical support**: Available resources

#### Amateur radio bulletin
```
T:ARES BULLETIN
M:TRANSMITTER|W1AW
M:FREQUENCY|3.950MHz
H:ACTIVITIES
P:DAILY NET 19H00 UTC
P:MORSE TRAINING TUESDAY
H:ALERTS
P:NO MAJOR ACTIVITY
P:RADIO WATCH MAINTAINED
```

## 10.6 Low-tech educational systems

### Education in isolated areas

#### School textbooks
- **Structured content**: Progressive lessons
- **Exercises**: Self-assessment
- **Progress tracking**: Individual students

#### Elementary course
```
T:MATHEMATICS GRADE 1
H:ADDITION
P:Addition is combining two groups.
C:2 + 3 = 5
C:4 + 1 = 5
H:MULTIPLICATION
P:Multiplication is repeated addition.
C:2 × 3 = 6
Q:Remember: multiplication is faster than addition!
```

## 10.7 Embedded technical documentation

### Equipment manuals

#### MML advantages
- **Reduced size**: Limited memory
- **Easy search**: Hierarchical structure
- **Update**: Independent fragments

#### Compressor manual
```
T:AIR COMPRESSOR MANUAL
M:MODEL|CA-200
M:VERSION|2.1
H:INSTALLATION
P:Place on stable horizontal surface.
P:Check 220V electrical supply.
H:MAINTENANCE
P:Drain condenser weekly.
P:Clean filter every 3 months.
C:# Emergency startup
C:if power_failure:
C:    switch_to_backup()
H:TROUBLESHOOTING
P:If no pressure: check fuse.
P:If abnormal noise: drain oil.
```

## 10.8 Universal signaling

### Cross-cultural communication

#### Universal signs
- **Danger**: World-recognized symbols
- **Help**: Standardized distress signals
- **Directions**: Visual guidance

#### MML sign
```
T:SITE SIGNALING
H:DANGER
M:TYPE|HIGH VOLTAGE
P:DO NOT TOUCH
P:MINIMUM DISTANCE 3 METERS
IMG:electric-danger-picto.png
H:ACCESS
P:ENTRY AUTHORIZED PERSONNEL ONLY
P:BADGE REQUIRED
L:ACCESS PROCEDURE|security-procedure.pdf
```

## 10.9 Automatic AI → MML conversions

### Assisted generation

#### AI as writer
- **Audio transcription** → Structured MML
- **Image analysis** → MML descriptions
- **Automatic summary** → Concise documents

#### Example: AI report
```
T:INCIDENT REPORT
M:GENERATED_BY|AI_ANALYSIS
M:CONFIDENCE|87%
H:DESCRIPTION
P:Car accident detected main intersection.
M:VEHICLES|2
M:INJURED|1 MINOR
H:PROBABLE CAUSE
P:Failure to yield right of way.
Q:Recommendation: install traffic light.
```

---

**Chapter conclusion**: MML use cases demonstrate its exceptional versatility. From the battlefield to classrooms, from polar expeditions to disaster zones, MML adapts to all contexts where communication must remain possible despite constraints. Each scenario reveals a different facet of the format's robustness and universality.

