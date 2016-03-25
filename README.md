Calendar Plugin for Mecha CMS
=============================

> Hook–able calendar widget.

This is the core widget. Add your own custom events by using the available hooks.

### Basic Usage

As plain widget:

~~~ .php
<?php echo Widget::calendar('your_unique_id'); ?>
~~~

As block widget:

~~~ .php
<?php echo Shield::chunk('block.widget', array(
    'title' => $speak->calendar,
    'content' => Widget::calendar('your_unique_id')
)); ?>
~~~

### Hooks

Your calendar ID will also affect the calendar hook ID. This way, you can add some notes to the specified date based on the calendar ID.

~~~ .php
Calendar::hook('your_unique_id', function($lot, $year, $month, $id) { … });
~~~

~~~ .php
Calendar::date('2016/4/21', 'your_unique_id', array( … ));
Calendar::date('2016/4', 'your_unique_id', array( … ));
Calendar::date('2016', 'your_unique_id', array( … ));
~~~

### Examples

Add a new event on `2016/4/21`:

~~~ .php
Calendar::date('2016/4/21', 'your_unique_id', array(
    'url' => '/article/happy-birth-day', // ← the URL you want to link to
    'description' => 'Today is my birthday.', // ← the event description
    'kind' => array('fun', 'party', 'me') // ← add some tags (optional)
));
~~~

More events:

~~~ .php
Calendar::hook('your_unique_id', function($lot) {
    $lot['2016/4/21'] = array('description' => 'This is a test 1.');
    $lot['2016/4/22'] = array('description' => 'This is a test 2.');
    $lot['2016/4/23'] = array('description' => 'This is a test 3.');
    // …
    return $lot;
});
~~~

Using files:

~~~ .php
Calendar::hook('your_unique_id', function($lot, $year, $month, $id) {
    $the_year = Request::get('calendar.' . $id . '.year', $year);
    $the_month = Request::get('calendar.' . $id . '.month', $month);
    if($the_year === $year && $the_month === $month) {
        $events = glob('path/to/' . $year . '/' . $month . '/[0-9]*.txt');
        foreach($events as $event) {
            $file = File::open($event)->unserialize();
            $lot[$year . '/' . $month . '/' . File::N($event)] = $file;
        }
    }
    return $lot;
});
~~~