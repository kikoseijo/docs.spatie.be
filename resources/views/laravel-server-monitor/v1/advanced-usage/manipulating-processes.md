---
title: Manipulating processes
---

Under the hood `Symfony\Component\Process` is used to connect to your server and performing the command from your check.  You can manipulate this `Process` right before it is executed.

To do this you must create that implements `Spatie\ServerMonitor\Manipulators\Manipulator`. That interface has only a singe method to implement:

```php
public function manipulateProcess(Process $process, Check $check): Process;
```

Let's take a look at an example implementation. In the code below we're going to change the command and up the timeout if the check is being performed on a certain host.

```php
namespace App\ServerMonitor\Manipulators;

use Spatie\ServerMonitor\Models\Check;
use Symfony\Component\Process\Process;

class MyManipulator implements Manipulator
{
    public function manipulateProcess(Process $process, Check $check): Process
    {
        if ($check->host->name = 'my-host') {
            $manipulatedCommand = "{$process->getCommandLine()} appending extra options";
            
            $process->setCommandLine($manipulatedCommand);
            
            $process->setTimeout(60);
        }
        
        return $process;
    }
}

```

After creating the class you must specify it's full qualified name in the `process_manipulator` key of the config file.