

Command line tools are powerful utilities that allow users to interact directly with the operating system through a text-based interface. For developers, data analysts, and engineers, mastering these tools can greatly enhance productivity and efficiency. One of the most common tasks performed using command line tools is manipulating CSV (Comma-Separated Values) files—a simple, yet widely used format for storing and exchanging tabular data.

In this guide, we'll explore how to leverage command line tools to efficiently work with CSV files. Whether you need to filter, sort, or transform large datasets, command line tools offer a range of functionalities that can simplify these tasks. By the end of this, you’ll have a solid understanding of how to harness the power of the command line to manage and manipulate CSV files, streamlining your workflow and enhancing your data processing capabilities.

Below, questions and their solutions are mentioned. I’ve also attached a csv file upon which I’ve worked.

1)	Display Specific Day's Schedule: 	Write a bash script( todays_schedule.sh ) that displays the schedule for today . 	Usage: todays_schedule.sh 

	todays_schedule.sh:

	today=$(date +%A)
	echo "Schedule for $today: "
	grep "^$today", timetable.csv


2)	Count Courses per Instructor: 	Write a bash script that counts the number of courses each instructor is teaching. 	Usage: instructor_course_count.sh

	instructor_course_count.sh 
	instructor name: MKR

	read inst_name
	echo "Courses taught by $inst_name: "
	grep -c $inst_name timetable.csv


3)	List Courses in a Specific Room: 	Write a bash script that lists all the courses that are held in a specific room. 	Usage: room_course_list.sh 

	room_course_list.sh
	room_name: CEP-207

	read room_no	
	echo "Courses held in $room_no: "	
	grep -c $room_no timetable.csv


4)	Extract Timetable for a Specific Course: 	Write a bash script that extracts and displays the timetable for a specific course. 	Usage: course_timetable.sh 

	course_timetable.sh
	course: SC223

	read course 
	echo "Selected Course: $course"
	grep $course timetable.csv


5) 	Find out when a room is empty during the week.  	Usage: room_empty_slots.sh CEP-207 

	room_empty_slots.sh CEP-205

	#!/bin/bash
	if [ -z "$1" ]; then
		echo "Usage: $0 ROOM_NAME"
		exit 1
	fi

	ROOM_NAME="$1"
	CSV_FILE="timetable.csv"

	if [ ! -f "$CSV_FILE" ]; then
		echo "Error: $CSV_FILE not found!"
		exit 1
	fi

	HEADER=$(head -n 1 "$CSV_FILE")


	TIMES=$(cut -d ',' -f 2 "$CSV_FILE" | sort | uniq)
	DAYS=$(cut -d ',' -f 1 "$CSV_FILE" | sort | uniq)

	echo "Empty slots for room $ROOM_NAME:"

	for day in $DAYS; do
		for time in $TIMES; do
			if ! grep -q "^$day,$time,.*,.*,${ROOM_NAME}$" "$CSV_FILE"; then
				echo "$day at $time"
			fi
		done
	done



6)	Count Classes per Room per Day: 	Write a bash script to count how many classes are held in each room for each day. 	Usage: room_class_count.sh 	Output: 
	Room, Day, Count
	CEP-207, Monday,3
	CEP-103, Tuesday,5

	room_class_count.sh
	
	#!/bin/bash

	CSV_FILE="timetable.csv"

	if [ ! -f "$CSV_FILE" ]; then
		echo "Error: $CSV_FILE not found!"
		exit 1
	fi

	ROOMS=$(cut -d ',' -f 5 "$CSV_FILE" | sort | uniq)
	DAYS=$(cut -d ',' -f 1 "$CSV_FILE" | sort | uniq)

	echo "Room,Day,Count"

	for room in $ROOMS; do
		for day in $DAYS; do
			count=$(grep "^$day,.*,.*,.*,${room}$" "$CSV_FILE" | wc -l)
				if [ "$count" -gt 0 ]; then
					echo "$room,$day,$count"
				fi
		done
	done


7)	Find out where an instructor would be at current time? 
	Display "office" in case not in class. 	Usage: instructor_location.sh instructor_name

	instructor_location.sh MKR

	#!/bin/bash

	CSV_FILE="timetable.csv"


	if [ -z "$1" ]; then
		echo "Usage: $0 INSTRUCTOR_NAME"
		exit 1
	fi

	INSTRUCTOR_NAME="$1"

	if [ ! -f "$CSV_FILE" ]; then
		echo "Error: $CSV_FILE not found!"
		exit 1
	fi

	CURRENT_DAY=$(date +"%A")
	CURRENT_TIME=$(date +"%H:%M")

	LOCATION=$(grep "^$CURRENT_DAY,$CURRENT_TIME,.*,${INSTRUCTOR_NAME},.*$" "$CSV_FILE" | cut -d ',' -f 5)

	if [ -n "$LOCATION" ]; then
		echo "$INSTRUCTOR_NAME is currently in room $LOCATION."
	else
		echo "$INSTRUCTOR_NAME is currently in their office."
	fi


The END.
















	
