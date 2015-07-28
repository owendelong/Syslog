About
===

WARNING -- WIP -- Work in Progress -- GITHUB doesn't allow me to mark libraries private without paying, so this
is public even though it isn't anywhere near functional yet. Ordinarily, I would mark private until ready then
make public.

DO NOT ATTEMPT TO USE THIS LIBRARY AT THIS TIME.

If you want to contribute code (or funding), please let me know.

This warning will be removed once there's an actual library here.

This library provides basic SYSLOG client functionality as an alternative to Serial logging on Particle.

Intent is to make SYSLOG almost as easy to use as Serial.print().

## Table of Contents

This README describes the general use of the library.

	- About -- Describes the library
	- Table of Contents -- This list
	- The bad news -- Talks about the limitations of the library
	- Library Details -- Primary man-page style documentation of the library
	- Example Usage -- An example for using the library

## The bad news

I'd love to have written a general library that would work across all Arduino-like microcontrollers with network connectivity.
Unfortunately, since the underlying network functions are so radically different across each and every different one, there's
really no generic networking library to base such a thing on.

I've written the library for the particle platform initially as that's where I need it the most. I will likely also port it
myself to the ESP8266 boards (specifically the Adafruit ESP8266 Huzzah breakout).

Porting to wired ethernet boards may require more effort. I've tried to stay as close to the Arduino generic WiFi library as possible.

As I have been unable to yet find a sufficiently cost-effective microcontroller with IPv6 support, there is no IPv6 support in this library
at this time.

## Library Details

```

#include "Syslog.h" /* If using Particle DEV or most other environments */
#include "Syslog/Syslog.h" /* If using build.particle.io Cloud IDE */

void setuplog(const char *loghost = "syslog.local", int logmask = LOG_EMERG|LOG_ALERT|LOG_CRIT|LOG_ERR, uint16_t port=514);
void openlog(const char *ident, int option, int facility);
void syslog(int priority, const char *format, ...);
void closelog(void);
void setlogmask(int logmask);

```

If setuplog() is called, it should be called before openlog(), or, the log should be closed prior to calling setuplog() and then re-opened.

The openlog() call must preceed any calls to syslog().
It will create a socket ready to send data to the specified syslog server.

` setuplog()
* loghost is the servername of the system where log entries should be posted. Should be a host name, but can be an IP address.
port is the port number to use for logging, defaults to 514. This library logs via UDP.
* logmask is the logical or of all desired log levels. Log messages that are not members of the mask will not be transmitted to the server.
> e.g. if logmask = LOG_EMERG|LOG_ALERT|LOG_NOTICE, then log messages with priority LOG_EMERG will be sent, but LOG_CRIT will not.

` openlog()
* ident is a sting which will be automatically prepended to log entries.
* option logical OR of any of the following:
  - LOG_CONS -- Log directly to console if there is an error while sending to logger.
  - LOG_NDELAY -- Open connection Immediately -- This option is meaningless in this implementation as the connection is always opened immediately
  - LOG_NOWAIT -- Don't wait() for chile processes created while logging -- This option is meaningless in this implementation as no child processes are created
  - LOG_ODELAY -- Converse of LOG_NDELAY -- Meaningless in this implementation, delay is not an option.
  - LOG_PERROR -- Log to STDERR as well -- In this implementation, will duplicate messages to Serial0 via Serial.write()
  - LOG_PID -- Include PID with each message -- Meaningless in this implementation, no process table.
* facility specifies what kind of program is logging the message. This is used by syslogd to route messages to the appropriate log destination.
  - LOG_AUTH -- security/authorization messages (DEPRECATED, Use LOG_AUTHPRIV instead)
  - LOG_AUTHPRIV -- security/authorization messages (private)
  - LOG_CRON -- clock daemon (cron and at)
  - LOG_DAEMON -- system daemons without separate facility value
  - LOG_FTP -- ftp daemon
  - LOG_KERN -- Kernel messages (these can't be generated from user processes)
  - LOG_LOCAL0 through LOG_LOCAL7 -- reserved for local use (most likely you want to use one of these)
  - LOG_LPR -- line printer subsystem
  - LOG_MAIL -- mail subsystem
  - LOG_NEWS -- USENET news subsystem
  - LOG_SYSLOG -- messages generated internally by syslogd
  - LOG_USER (default) -- Generic user-level messages
  - LOG_UUCP -- UUCP subsystem

` syslog()
* priority is exactly one of the following:
  - LOG_EMERG -- System is unusable
  - LOG_ALERT -- Action must be taken immediately
  - LOG_CRIT -- Critical conditions
  - LOG_ERR -- Error conditions
  - LOG_WARNING -- Warning conditions
  - LOG_NOTICE -- normal, but significant, condition
  - LOG_INFO -- Informational message
  - LOG_DEBUG -- Debug-level message

` setlogmask()
* logmask is the logical OR of one or more values from priority above.
* Messages of levels corresponding to a 0 in the current logmask are not transmitted.
* Messages of levels corresponding to a 1 in the current logmask are transmitted.
* setlogmask() can be called at any time to change the logmask.


` closelog()
* Closes out the socket and frees up resources


## Example Usage

```

#include "Syslog.h"

void setup {
  /* Any leading initialization code */
  #ifdef DEBUG
    setuplog("loghost.example.com", LOG_EMERG|LOG_ALERT|LOG_CRIT|LOG_ERR|LOG_WARNING|LOG_NOTICE|LOG_INFO|LOG_DEBUG)
  #else
    setuplog("loghost.example.com")  /* Equivalent to: LOG_EMERG|LOG_ALERT|LOG_NOTICE
  #endif
  openlog("Example Program", LOG_PERROR, LOG_LOCAL4);
  /* more initialization code here */
  syslog(LOG_INFO, "%s", "Initialization complete");
}


void loop {
  char desc[] = "my_function_name";
  int result;
  int errno = 129;
  extern char *sys_errlist[];
  /* Some code */
  result = my_function_name();
  if (result == R_ERROR)
  {
    syslog(LOG_ERR, "%s: resulted in error number %d -- %s", desc, errno, sys_errlist[errno]);
  }


  /* more code */

  if (time_to_idle)
  {
    closelog()
    systemSleep(DEEP_SLEEP, 30);
  }
}


```

