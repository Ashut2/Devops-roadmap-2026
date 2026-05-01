## What I Fixed  Today 
- Originally the content was generalised , made a little more personalised
- Add some logos for dynamic look.

## What I learned Today
`warning: in the working copy of 'portfolio/yarn.lock', LF will be replaced by CRLF the next time Git touches it`

This is a harmless Windows/Linux line ending warning. Let me explain:

What are LF and CRLF?
When you press Enter, computers save it differently:

Linux/Mac uses LF (Line Feed) = \n
Windows uses CRLF (Carriage Return + Line Feed) = \r\n


What's happening here?
Your yarn.lock was generated in WSL (Linux) so it uses LF. But Git on Windows is configured to auto-convert to CRLF.
So Git is saying:

"Hey, this file has Linux line endings. When I save it next time, I'll convert to Windows style."


Does it break anything?
No. our code works fine. our CI/CD pipeline runs on Linux so it's fine here too.

To silence the warning permanently:
bashgit config --global core.autocrlf true
This tells Git "yes I know, I'm on Windows, handle it automatically" — and the warning goes away.

DevOps takeaway: This is why in real teams, projects have a .gitattributes file that enforces consistent line endings across all machines. Something like:
`* text=auto
*.sh text eol=lf`
