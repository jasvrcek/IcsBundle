IcsBundle
=========

Symfony Bundle providing dependency injection for the Jsvrcek\ICS library, which is for creating iCal .ics files.

## Installation

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

    $attendee = new Attendee(new Formatter);

you can instead, in your Symfony controller, use:

    namespace Acme\HelloBundle\Controller;

    class HelloController
    {
        public function calendarAction()
        {
            $attendee = $this->get('jsvrcek_ics.model.relationship.attendee');
            ...
            
            $calendarExport = $this->get('jsvrcek_ics.export');
            ...
            
            $response = new Response($calendarExport->getStream());
            $response->headers->set('Content-Type', 'text/calendar');
            
            return $response;
        }
    }
