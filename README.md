IcsBundle
=========

Symfony Bundle providing dependency injection for the Jsvrcek\ICS library, which is for creating iCal .ics files.

## Installation

```bash
composer req jsvrcek/ics-bundle
```

## Usage

```php
    namespace App\Services;

    use Jsvrcek\ICS\CalendarExport;
    use Jsvrcek\ICS\Model\Calendar;
    use Jsvrcek\ICS\Model\CalendarEvent;
    use Jsvrcek\ICS\Model\Relationship\Attendee;
    use Jsvrcek\ICS\Utility\Formatter;

    private Formatter $formatter;
    private CalendarExport $calendarExport;

    class MyService
    {

    public function __construct(Formatter $formatter, CalendarExport $calendarExport) {

        $this->formatter = $formatter;
        $this->calendarExport = $calendarExport;
    }
```    


    // or inject them into the controller
```php

        public function calendarAction(Formatter $formatter, CalendarExport $calendarExport)
        {
            $eventOne = new CalendarEvent();
            $eventOne->setStart(new \DateTime())
                ->setSummary('Family reunion')
                ->setUid('event-uid');

            //add an Attendee
            $attendee = new Attendee($formatter);
            $attendee->setValue('moe@example.com')
                ->setName('Moe Smith');
            $eventOne->addAttendee($attendee);

            // Add event to CalendarExport
            $calendarExport->setCalendars( [
                (new Calendar)->setTimezone( new DateTimeZone(date_default_timezone_get()) )
                    ->addEvent( $eventOne )
            ] );
            
            $response = new Response($calendarExport->getStream());
            $response->headers->set('Content-Type', 'text/calendar');
            
            return $response;
        }
    }
