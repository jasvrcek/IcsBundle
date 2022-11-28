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
        $attendee = new Attendee($this->formatter); // or $formatter
        $attendee->setValue('moe@example.com')
            ->setName('Moe Smith');
        $eventOne->addAttendee($attendee);

            
            $response = new Response($calendarExport->getStream());
            $response->headers->set('Content-Type', 'text/calendar');
            
            return $response;
        }
    }
