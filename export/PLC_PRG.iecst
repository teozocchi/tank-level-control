VAR
	rLevel : REAL := 8.0; // tank lvl 0-100
	rSP_Low : REAL := 30.0; // low setpoint
	rSP_High : REAL := 80.0; // hi setpoint
	rSP_AlarmHigh : REAL := 90.0; // highest setpoint
	rSp_AlarmLow : REAL := 10.0;
	xInletValve : BOOL; // TRUE when inlet open
	xPump : BOOL; // TRUE when outlet pump active
	xValveClosed : BOOL;   // TRUE when inlet valve closed
	xPumpStopped : BOOL;   // TRUE when pump stopped
	xStop: BOOL; // stop btn
	xStart: BOOL; // start btn
	xStartPrev : BOOL;
	xStopPrev  : BOOL;
	xFilling : BOOL;
	xAutoMode: BOOL := TRUE; // TRUE=auto, FALSE=manual
	xManualMode: BOOL := FALSE;
	xManInlet: BOOL; // manual inlet cmd
	xManPump : BOOL; // manual pump cmd
	xRunning : BOOL; //running latch
	xHighAlarm : BOOL; // hi lvl alarm
	xLowCutout: BOOL; // dry run protection
	xHighAlarmOk : BOOL;   // TRUE when high alarm NOT active (normal)
	xLowCutoutOk : BOOL;   // TRUE when low cutout NOT active (normal)
	rInflow: REAL;
	rOutflow: REAL;
	xAllOk : BOOL := TRUE;
	xNotOk: BOOL := FALSE;
END_VAR

// start/stop latch (edge-detected)
IF xStart AND NOT xStartPrev THEN xRunning := TRUE; END_IF
IF xStop AND NOT xStopPrev THEN xRunning := FALSE; END_IF
xStartPrev := xStart;
xStopPrev  := xStop;

// control
IF xRunning = TRUE AND xAutoMode = TRUE THEN
    IF rLevel <= rSP_Low THEN xFilling := TRUE; END_IF   // fill 
    IF rLevel >= rSP_High THEN xFilling := FALSE; END_IF   // drain
    xInletValve := xFilling;
    xPump := NOT xFilling;          // never sotps mid band

ELSIF xRunning = TRUE AND xAutoMode = FALSE THEN
    xInletValve := xManInlet; xPump := xManPump;

ELSE
    xInletValve := FALSE; xPump := FALSE;
END_IF

// alarms
xHighAlarm := rLevel >= rSP_AlarmHigh;
xLowCutout := rLevel <= rSp_AlarmLow;
//IF xRunning AND xHighAlarm THEN xInletValve := FALSE; xPump := TRUE; END_IF
//IF xRunning AND xLowCutout THEN xInletValve := TRUE;  xPump := FALSE; END_IF

// physics simulation
IF xInletValve = TRUE THEN rInflow := 2.0; ELSE rInflow := 0.0; END_IF
IF xPump = TRUE THEN rOutflow := 1.5; ELSE rOutflow := 0.0; END_IF
rLevel := (rLevel + rInflow) - rOutflow;

IF rLevel > 100.0 THEN rLevel := 100.0; END_IF
IF rLevel < 0.0 THEN rLevel := 0.0; END_IF

xHighAlarmOk := NOT xHighAlarm; // for visualization
xLowCutoutOk := NOT xLowCutout; // for visualization
xValveClosed := NOT xInletValve; // for visualization
xPumpStopped := NOT xPump; // for visualization
IF xLowCutoutOK = TRUE AND xHighAlarmOk = TRUE THEN xAllOk := TRUE; ELSE xAllOk := FALSE; END_IF // for visualization
IF xAutoMode = TRUE THEN xManualMode := FALSE; ELSE xManualMode := TRUE; END_IF // for visualization