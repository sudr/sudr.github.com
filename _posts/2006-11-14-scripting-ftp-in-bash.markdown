---
layout: post
title: Scripting FTP in BASH
---

The following code snippet can be embedded in a bash script to ftp a file or script other ftp functions. It also includes error checking if the ftp of the file fails.


		function ftpfile {
			# failure or success flag
			FLAG=0 # assume success
			# Start the actual ftp of the file
			RETUR=$(ftp -n <>${LOGFILE}
				open $SERVER
				user $USERNAME $PASSWORD
				ascii
				put file.txt
				bye
				EOF
			)

			# End of the actual ftp process
			if [ "$RETUR" != "" ]
			then
				FLAG=1
			fi
			return $FLAG		
		}
		# END FTP FUNCTION
		
		# call function to ftp file
		ftpfile $file
		FLAG=$?
		if [ "$FLAG" -eq "0" ]
		then
			echo "------> File transfer completed" 
		else 
			echo "------x File transfer failed"
		fi		