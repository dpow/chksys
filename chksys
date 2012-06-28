#!/usr/local/bin/bash
#
# chksys - Basic filesystem security check.
#          Manages permissions and checksums separately for the time being.
#          Generates list of and verifies ownership, permissions, and 
#          sha1sums of critical system files.
# Author:  Dylan Powell, (C) Copyright 2012
# License: Beerware, see LICENSE for details
#


# --------------------
# Secure Yo' Environz
# --------------------
umask 077			# No-one gets rwx but me!
\unalias -a			# Unset all aliases
hash -r				# Clear the command path hash
ulimit -H -c 0 --		# Set the hard limit to 0 to turn off core dumps
IFS=$' \t\n'			# Set a sane/secure IFS (NOTE: bash & ksh93 syntax only!)



# -----------------
# Global Variables
# -----------------
PERMS_LOG_FILE="/var/log/perms.log"
PERMS_DIFF_FILE="/var/log/perms.diff"
SUMS_LOG_FILE="/var/log/sums.log"
SUMS_DIFF_FILE="/var/log/sums.diff"

SUMS_SRC_DIRS='/bin/* /boot/* /etc/* $HOME/* $HOME/bin/* /root /sbin/* /usr/include/* /usr/local/* /usr/local/bin/* /usr/local/etc/* /usr/src/* /var/cron/* /var/log/*'
PERMS_SRC_DIRS='/bin /boot /etc $HOME $HOME/bin /root /sbin /usr/include /usr/local /usr/local/bin /usr/local/etc /usr/src /var/cron /var/log'

declare -a PERMSRC	# Array for permissions source directories

# Split $PERMS_SRC_DIRS into separate fields for easy parsing within the array
PERMSRC=($(echo $PERMS_SRC_DIRS | awk '{ for ( i = 1; i <= NF; i++) print $i }'))



# ----------
# Functionz
# ----------
usage() {
	echo "Usage:  chksys record     (record current system state)" >&2	
	echo "        chksys verify     (verify current system state)" >&2
	echo "        chksys backup     (back up current logs)" >&2
	echo "        chksys dump       (empty current logs)" >&2
	echo "        chksys help       (print this help message)" >&2
}

backup_logs() {
	if [ ! -e $PERMS_LOG_FILE ]; then
		echo "Error $0:  $PERMS_LOG_FILE not found." >&2
		echo "Time to dIIIIIIIIIEEEEEEEEEEE" >&2
		exit 1
	elif [ ! -e $SUMS_LOG_FILE ]; then
		echo "Error $0:  $SUMS_LOG_FILE not found." >&2
		echo "Time to dIIIIIIIIIEEEEEEEEEEE" >&2
		exit 1
	else	
		cp $PERMS_LOG_FILE $PERMS_LOG_FILE.bak && chmod go-rwx $PERMS_LOG_FILE.bak
		cp $SUMS_LOG_FILE $SUMS_LOG_FILE.bak && chmod go-rwx $SUMS_LOG_FILE.bak
	fi
}

dump_logs() {
	if [ ! -e $PERMS_LOG_FILE ]; then
		touch $PERMS_LOG_FILE
	elif [ ! -e $SUMS_LOG_FILE ]; then
		touch $SUMS_LOG_FILE
	else
		cp /dev/null $PERMS_LOG_FILE
		cp /dev/null $SUMS_LOG_FILE
	fi
}

dump_diffs() {
	if [ ! -e $PERMS_DIFF_FILE ]; then
		touch $PERMS_DIFF_FILE
	elif [ ! -e $SUMS_DIFF_FILE ]; then
		touch $SUMS_DIFF_FILE
	else
		cp /dev/null $PERMS_DIFF_FILE
		cp /dev/null $SUMS_DIFF_FILE
	fi
}

record_fs_state() {
	# Get and store the permissions we want
	for i in "${PERMSRC[@]}"
	do
		cd $i && ls -la | awk '{print $1,$3,$4,$NF}' >> $PERMS_LOG_FILE
	done

	# Compute the checksums of files we want to monitor
	for f in $SUMS_SRC_DIRS; 
	do
		sha1 -r $f >> $SUMS_LOG_FILE
	done
}

get_new_fs_state() {
	# Get the current permissions
	for i in "${PERMSRC[@]}"
	do
		cd $i && ls -la | awk '{print $1,$3,$4,$NF}' >> $PERMS_DIFF_FILE
	done

	# Compute the current file checksums
	for f in $SUMS_SRC_DIRS; 
	do
		sha1 -r $f >> $SUMS_DIFF_FILE
	done
}

verify_fs_state() {
	diff $PERMS_LOG_FILE $PERMS_DIFF_FILE
	diff $SUMS_LOG_FILE $SUMS_DIFF_FILE
}



# -----------------
# Argument Parsing
# -----------------
while [ $# != 0 ]
do
	case $1 in 
		record)
			backup_logs
			dump_logs
			dump_diffs
			record_fs_state
			exit 0
			;;
		verify)
			dump_diffs
			get_new_fs_state
			verify_fs_state
			exit 0
			;;
		backup)
			backup_logs
			exit 0
			;;
		dump)
			dump_logs
			dump_diffs
			exit 0
			;;
		-h|--h|--he|--hel|--help|help)
			usage
			exit 0
			;;
		*)
			echo "chksys:  invalid option." >&2
			usage
			exit 1
			;;
	esac
	shift
done
