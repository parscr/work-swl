## Configure Local Pasword Policiy 

Requirement 1 Password History 1

Requirement 2 Max Password age 365

Requirement 3 Min Password age 0

Requirement 4 Min Password Length 8

Requirement 5 Contains characeters from 3 of the 4 (Upper;Lower;Digit;Non Alphabectic)

Requirement 6 Not contain user names excced two

### Requirement 1 Password History

vim /etc/pam.d/system-auth and /etc/pam.d/password-auth (after pam_pwquality.so line):
```
password    requisite     pam_pwhistory.so remember=1 use_authtok
```

### Requirement 2,3 Configure the minimum & maximum number of days that a password can be used 0/365

vim /etc/login.defs
```
#Password aging controls:

#PASS_MAX_DAYS	Maximum number of days a password may be used.
#PASS_MIN_DAYS	Minimum number of days allowed between password changes.
#PASS_MIN_LEN	Minimum acceptable password length.
#PASS_WARN_AGE	Number of days warning given before a password expires.
#
PASS_MAX_DAYS	365
PASS_MIN_DAYS	0
PASS_MIN_LEN	5
PASS_WARN_AGE	7
```

### Requirement 4,5 & 6 Password complexity

vim /etc/security/pwquality.conf

```json
# Configuration for systemwide password quality limits
# Defaults:
#
# Number of characters in the new password that must not be present in the
# old password.
# difok = 5
#
# Minimum acceptable size for the new password (plus one if
# credits are not disabled which is the default). (See pam_cracklib manual.)
# Cannot be set to lower value than 6.
minlen = 9
#
# The maximum credit for having digits in the new password. If less than 0
# it is the minimum number of digits in the new password.
dcredit = 0
#
# The maximum credit for having uppercase characters in the new password.
# If less than 0 it is the minimum number of uppercase characters in the new
# password.
ucredit = 0
#
# The maximum credit for having lowercase characters in the new password.
# If less than 0 it is the minimum number of lowercase characters in the new
# password.
lcredit = 0
#
# The maximum credit for having other characters in the new password.
# If less than 0 it is the minimum number of other characters in the new
# password.
ocredit = 0
#
# The minimum number of required classes of characters for the new
# password (digits, uppercase, lowercase, others).
minclass = 4
#
# The maximum number of allowed consecutive same characters in the new password.
# The check is disabled if the value is 0.
# maxrepeat = 0
#
# The maximum number of allowed consecutive characters of the same class in the
# new password.
# The check is disabled if the value is 0.
# maxclassrepeat = 0
#
# Whether to check for the words from the passwd entry GECOS string of the user.
# The check is enabled if the value is not 0.
# gecoscheck = 0
#
# Path to the cracklib dictionaries. Default is to use the cracklib default.
# dictpath =
```

Save & Quit

NOTES: Credits!

pam_pwquality uses a "scoring" system that combines password length requirements with a "credit" system based on the number of different types of characters.

You start with the minlen=N parameter which sets the minimum acceptable length for a password. However, the user gets one "credit" each for using a lower-case letter, an upper-case letter, a number, and a punctuation character. So if minlen=15, the user could still use an 11 character password if it contained all four character classes.

pam_pwquality also supports a minclass=N parameter that requires characters from at least N of the four different character classes. This is probably a better way to go than specifically requiring a specific type of character.

### Minimum Password Length.
To change the minimum length, do two things:

Remove the hash (#) character from the beginning of the line
Change the length to your desired length

```
# Minimum acceptable size for the new password (plus one if
# credits are not disabled which is the default). (See pam_cracklib manual.)
# Cannot be set to lower value than 6.
#minlen = 8
minlen 10
```

Change the following parameters to “0” and Remove the hash (#) for them as well
```
dcredit = 0
ucredit = 0
lcredit = 0
ocredit = 0
```

Above we disabled “credits”, which gives passwords points for things like digits, capital letters, and characters different. The default for those was “1”, which means that each time you use one of these characters, the score is added to the length.

