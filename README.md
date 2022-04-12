IcsBundle
=========

Symfony Bundle providing dependency injection for the Jsvrcek\ICS library, which is for creating iCal .ics files.

## Installation

composer config repositories.ics_bundle '{"type": "vcs", "url": "git@github.com:tacman/IcsBundle.git"}'
composer req jsvrcek/ics-bundle:dev-tac

Add on composer.json (see http://getcomposer.org/)

    "require" :  {
        // ...
        "jsvrcek/ics-bundle":"*",
    }

## Usage

See https://github.com/jasvrcek/ICS/blob/master/README.md

This bundle integrates the Jsvrcek\ICS library with the Symfony dependency injection component, so instead of the example in the above doc:

    use Jsvrcek\ICS\Model\Relationship\Attendee;
    use Jsvrcek\ICS\Utility\Formatter;

    $attendee = new Attendee(new Formatter());

you can instead, in your Symfony controller, use:

    namespace Acme\HelloBundle\Controller;
    private Formatter $formatter;
    private CalendarExport $calendarExport;

    class HelloController
    {

    public function __construct(Formatter $formatter, CalendarExport $calendarExport) {

        $this->formatter = $formatter;
        $this->calendarExport = $calendarExport;
    }


    // or inject them into the controller

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
