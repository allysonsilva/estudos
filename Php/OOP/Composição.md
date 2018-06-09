# Composição Em Php

> É uma relação *Todo/Parte* como na agregação. A diferença é que na agregação o objeto *Todo* Pode existe sem o *Parte* e Vice-versa. Já na **composição** o objeto *Parte* so existe se o *Todo* existir. Por exemplo em um corpo humano, os dedos(*Parte*) compõem as mãos(*Todo*), logo se não existir mão então também não existirá dedos. Na **composição** ao destruir o objeto-pai(*Todo*), logo o objeto-filho(*Parte*) também será automaticamente destruido, pelo motivo de que na composição o objeto *Todo* é usado tanto para construir como para destruir.

## Implementando Composição

```php
class Client
{
    private $id;
    private $name;
    private $contact;
    private $data;

    public function __construct()
    {
        $this->contact = new Contact();
    }

    public function __set(string $name, $value): void
    {
        $this->data[$name] = $value;
    }

    public function __get(string $name)
    {
        if (array_key_exists($name, $this->data)) {
            return $this->data[$name];
        }
    }

    public function setContact($phone, $email)
    {
        $this->contact->setContact($phone, $email);
    }

    public function getContact()
    {
        return $this->contact->getContact();
    }
}

final class Contact
{
    private $phone;
    private $email;

    public function setContact(string $phone, string $email): void
    {
        $this->phone = $phone;
        $this->email = $email;
    }

    public function getContact(): string
    {
        return "Phone : {$this->phone}, Email : {$this->email}";
    }
}

$client = new Client();
$client->id = 1;
$client->name = 'Allyson Silva';

/*
class Client#1 (4) {
  private $id =>
  NULL
  private $name =>
  NULL
  private $contact =>
  class Contact#2 (2) {
    private $phone =>
    NULL
    private $email =>
    NULL
  }
  private $data =>
  array(2) {
    'id' =>
    int(1)
    'name' =>
    string(13) "Allyson Silva"
  }
}
*/
var_dump($client);

$client->setContact('85 91234-5678', 'email@mail.com');

/* string(45) "Phone : 85 91234-5678, Email : mail@email.com" */
var_dump($client->getContact());
```
