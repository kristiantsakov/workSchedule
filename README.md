# workSchedule
schedule
<?php
/**
 * Created by PhpStorm.
 * User: kristian
 * Date: 6/29/15
 * Time: 4:07 PM
 */
$day_start = 7;
$day_end = 19;
$positions = 4;
$people = 8;
$positions_hours = 8;
$work_hours = 20;
$busy = 5;
$day_hours = $day_end - $day_start;

function personSchedule($person,$busy,$personSchedule){
    global $day_start;
    global $day_end;
    global $positions;
    $personSchedule = array();
    for ($currentPosition = 0; $currentPosition < $positions;$currentPosition++ ) {
        for ($currentDay = 0; $currentDay < 5 ;$currentDay++ ) {
            for ($currentHour = $day_start; $currentHour < $day_end ;$currentHour++ ) {
                if($currentDay === $busy[$person][0] && ($busy[$person][1] < $currentHour && $currentHour < $busy[$person][2])){
                    $personSchedule[$currentPosition][$currentDay][$currentHour] = 1;
                }
                $personSchedule[$currentPosition][$currentDay][$currentHour] = 0;
            }
        }
    }
    return $personSchedule;
}

function algorithm($day_start,$day_end,$positions,$people,$position_hours,$work_hours,$busy)
{
    $day_start = 7;
    $day_end = 19;
    $positions = 4;
    $people = 8;
    $positions_hours = 8;
    $work_hours = 20;
    $busy = [[[0, 7, 11], [1, 11, 14]], [[2, 7, 9], [4, 7, 19]], [[2, 12, 14], [1, 15, 19], [0, 14, 17]], [[3, 12, 16], [2, 14, 17], [1, 15, 18]], [], [[0, 13, 15], [0, 7, 9], [4, 7, 11]], [4, 16, 19], [2, 7, 14]];
    $sortedBusy = array();

    for ($person = 0; $person < count($busy); $person++) {
        $busyHours = 0;
        if (isset($busy[$person][0][0])) {
            for ($busyDay = 0; $busyDay < count($busy[$person]); $busyDay++) {
                $busyHours += $busy[$person][$busyDay][2] - $busy[$person][$busyDay][1];
            }
            $busy[$person]["busyHours"] = $busyHours;
        } else {
            $busyHours = $busy[$person][2] - $busy[$person][1];
            $busy[$person]["busyHours"] = $busyHours;
        }
    }

    $counter = count($busy);
    for ($k = 0; $k < $counter; $k++) {
        $count = 0;
        for ($i = 0; $i < $counter; $i++) {
            if ($count < $busy[$i]["busyHours"]) {
                $count = $busy[$i]["busyHours"];
                $counters = $i;
            }
            if ($i === 7) {
                array_push($sortedBusy, $busy[$counters]);
                unset($busy[$counters]);
            }
        }
    }

    $day_hours = $day_end - $day_start;
    $schedule = array();
    var_dump($sortedBusy);
    //7,14,9,10,0,8,3,7
    for ($i = 1; $i <= $people; $i++) {
        for ($currentPosition = 0; $currentPosition < $positions; $currentPosition++) {
            for ($currentDay = 0; $currentDay < 5; $currentDay++) {
                for ($currentHour = 0; $currentHour < $day_hours; $currentHour++) {
                    if ($currentDay === $sortedBusy[$person][0] && ($sortedBusy[$person][1] < $currentHour && $currentHour < $sortedBusy[$person][2])) {
                        continue;
                    }
                    $schedule[$currentPosition][$currentDay][$currentHour] = $i;
                }
            }
        }
    }
    var_dump($schedule);

}

algorithm($day_start,$day_end,$positions,$people,$positions_hours,$work_hours,$busy);
