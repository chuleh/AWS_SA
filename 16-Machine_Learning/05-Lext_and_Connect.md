# Lex + Connect

* Lex
  * same tech that powers alexa:
    * Automatic Speech Recognition (ASR) to convert text to speech
    * natural language understanding to recognize the intent of text, callers
    * helps build chatbots, call center bots
* Connect
  * receive calls, create contact flows, cloud-based virtual contact center
  * can integrate with other CRM (customer relationship systems) or AWS services
  * no upfront payments - 80% cheaper than traditional contact centers solutions

* eg: phone call schedule an appointment
  * phone call => connect
  * connect streams to Lex (intent recognition)
  * Lex invokes lambda => schedules call on some CRM
