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
$day_hours = $day_end - $day_start;

function personSchedule($person,$busy){
    global $day_start;
    global $day_end;
    $personSchedule = array();
    if(isset($busy[$person][0][0])){
    $busyDays = count($busy[$person])-1;
        for ($busyDay = 0; $busyDay < $busyDays; $busyDay++){
            for ($currentDay = 0; $currentDay < 5 ;$currentDay++ ) {
                for ($currentHour = $day_start; $currentHour < $day_end ;$currentHour++ ) {
                    if($personSchedule[$currentDay][$currentHour] != 1) {
                        if ($currentDay == $busy[$person][$busyDay][0]) {
                            if ($currentHour >= $busy[$person][$busyDay][1] && $currentHour < $busy[$person][$busyDay][2]) {
                                $personSchedule[$currentDay][$currentHour] = 1;
                            } else {
                                $personSchedule[$currentDay][$currentHour] = 0;
                            }
                        }
                        if ($currentDay != $busy[$person][$busyDay][0]) {
                            $personSchedule[$currentDay][$currentHour] = 0;
                        }
                    }
                }
            }
        }
    } else {
        for ($currentDay = 0; $currentDay < 5 ;$currentDay++ ) {
            for ($currentHour = $day_start; $currentHour < $day_end ;$currentHour++ ) {
                if($currentDay == $busy[$person][0]){
                    if($currentHour >= $busy[$person][1] && $currentHour <= $busy[$person][2]){
                        $personSchedule[$currentDay][$currentHour] = 1;
                    }else{
                        $personSchedule[$currentDay][$currentHour] = 0;
                    }
                }elseif ($currentDay != $busy[$person][0]){
                    $personSchedule[$currentDay][$currentHour] = 0;
                }
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
    $busy = [[[0, 7, 12], [1, 11, 15]], [[2, 7, 14], [4, 7, 19]], [[2, 12, 15], [1, 15, 19], [0, 14, 18]], [[3, 12, 16], [2, 13, 17], [1, 15, 18]], [3,7,19], [[0, 13, 15], [0, 7, 9], [4, 7, 11]], [4, 16, 19], [2, 7, 14]];
    $busy2 = [[], [], [], [], [], [], [], []];
    $busy3 = [[[0, 7, 12], [1, 11, 19]], [[2, 7, 14], [4, 7, 19]], [[2, 12, 15], [1, 15, 19], [0, 14, 18]], [[3, 12, 16], [2, 13, 17], [1, 15, 18]], [3,7,19], [[0, 13, 15], [0, 7, 9], [4, 7, 11]], [4, 16, 19], [2, 7, 14]];
    $busy4 = [[[0, 7, 12], [1, 11, 15]], [[2, 7, 14], [4, 7, 19]], [[2, 12, 15], [1, 15, 19], [0, 14, 18]], [[3, 12, 16], [2, 13, 17], [1, 15, 18]], [3,7,19], [[0, 13, 15], [0, 7, 9], [4, 7, 11]], [4, 16, 19], [2, 7, 14]];

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

    for ($i = 0; $i < $people; $i++) {
        for ($currentPosition = 0; $currentPosition < $positions; $currentPosition++) {
            for ($currentDay = 0; $currentDay < 5; $currentDay++) {
                for ($currentHour = $day_start; $currentHour < $day_end; $currentHour++) {
                    $schedule[$currentPosition][$currentDay][$currentHour] = "";
                }
            }
        }
    }

    for ($i = "a",$worker = 0; $i < "i" ; $i++, $worker++) {
        $personHours = 0;
        $workerBusySchedule = personSchedule($worker,$sortedBusy);
        for ($currentPosition = 0; $currentPosition < $positions; $currentPosition++) {
            for ($currentDay = 0; $currentDay < 5; $currentDay++) {
                for ($currentHour = $day_start; $currentHour < $day_end; $currentHour++) {
                    if($workerBusySchedule[$currentDay][$currentHour]==1){
                        continue;
                    }else{
                        if(strlen($schedule[$currentPosition][$currentDay][7])+strlen($schedule[$currentPosition][$currentDay][8])+strlen($schedule[$currentPosition][$currentDay][9])
                            +strlen($schedule[$currentPosition][$currentDay][10])+strlen($schedule[$currentPosition][$currentDay][11])+strlen($schedule[$currentPosition][$currentDay][12])
                            +strlen($schedule[$currentPosition][$currentDay][13])+strlen($schedule[$currentPosition][$currentDay][14])+strlen($schedule[$currentPosition][$currentDay][15])+
                            strlen($schedule[$currentPosition][$currentDay][16])+strlen($schedule[$currentPosition][$currentDay][17])+strlen($schedule[$currentPosition][$currentDay][18])>=8){
                            continue 2;
                        }
                        if ($schedule[$currentPosition][$currentDay][$currentHour] != ""){
                            continue;
                        } else {
                            $schedule[$currentPosition][$currentDay][$currentHour] = $i;
                            $personHours++;
                            if($personHours >= 20){
                                continue 4;
                            }
                        }
                    }
                }
            }
        }
    }
    $output = "<table border='1px solid black'><tr>";
    $output .= "<th>DAYS</th><th colspan='12'>First Position</th><th></th><th colspan='12'>Second Position</th><th></th><th colspan='12'>Third Position</th><th></th><th colspan='12'>Fourth Position</th></tr><tr>";
    for ($currentDay = 0; $currentDay < 5; $currentDay++) {
        $output .= "<th>$currentDay</th>";
        for ($currentPosition = 0; $currentPosition < $positions; $currentPosition++) {
            for ($currentHour = $day_start; $currentHour < $day_end; $currentHour++) {
                if($schedule[$currentPosition][$currentDay][$currentHour]=="") {
                    $output .= "<td> * </td>";
                } else {
                    $output .= "<td>" . $schedule[$currentPosition][$currentDay][$currentHour] . "</td>";
                }
            }
            $output .= "<th></th>";
        }
        $output .= "</tr>" ;
    }
    $output .= "<tr><th></th><th>7-8</th><th>8-9</th><th>9-10</th><th>10-11</th><th>11-12</th><th>12-13</th><th>13-14</th><th>14-15</th><th>15-16</th><th>16-17</th><th>17-18</th><th>18-19</th>";
    $output .= "<th></th><th>7-8</th><th>8-9</th><th>9-10</th><th>10-11</th><th>11-12</th><th>12-13</th><th>13-14</th><th>14-15</th><th>15-16</th><th>16-17</th><th>17-18</th><th>18-19</th>";
    $output .= "<th></th><th>7-8</th><th>8-9</th><th>9-10</th><th>10-11</th><th>11-12</th><th>12-13</th><th>13-14</th><th>14-15</th><th>15-16</th><th>16-17</th><th>17-18</th><th>18-19</th>";
    $output .= "<th></th><th>7-8</th><th>8-9</th><th>9-10</th><th>10-11</th><th>11-12</th><th>12-13</th><th>13-14</th><th>14-15</th><th>15-16</th><th>16-17</th><th>17-18</th><th>18-19</th>";
    $output .= "</table>";
    echo $output;

    /*for ($currentPosition = 0; $currentPosition < $positions; $currentPosition++) {
        for ($currentDay = 0; $currentDay < 5; $currentDay++) {
            for ($currentHour = $day_start; $currentHour < $day_end; $currentHour++) {
                if($schedule[$currentPosition][$currentDay][$currentHour]=="") {
                    echo " *";
                } else {
                    echo " " . $schedule[$currentPosition][$currentDay][$currentHour] . " ";
                }
            }
            echo '<br>';
        }
        echo "<br>" ;
    }*/

}

algorithm($day_start,$day_end,$positions,$people,$positions_hours,$work_hours,$busy);
