# PUPPET-ICINGA2

### Vytvorenie objektu typu _host_
K zavedeniu stroja do monitorovania sa používa definovaný resource `icinga2::host` s nasledujúcimi atribútmi:
#### **`Povinné`** atribúty:

| Názov atribútu        | Typ             | Popis atribútu           |
|-----------------------|-----------------|--------------------------|
|address                | String          | reprezentuje IPv4 stroja |

#### Voliteľné atribúty:

| Názov atribútu        | Typ             |Defaultná hodnota  | Popis atribútu                                                                                             |
|-----------------------|-----------------|-------------------|------------------------------------------------------------------------------------------------------------|
| enable_active_checks  | Boolean         |true               | Povolenie aktívnych kontrol                                                                                |
| enable_event_handle   | Boolean         |true               | Povolenie obsluhy události                                                                                 |
| enable_notifications  | Boolean         |false              | Povolenie notifikácií                                                                                      |
| groups                | Array[String]   |[]                 | Zoznam _host groups_                                                                                       |
| check_command         | String          | "hostalive"       | Názov kontrolného príkazu                                                                                  |
| check_interval        | Integer         | 300               | Interval kontroly v sekundách. Tento interval sa použije když je _host_ v stave  `HARD`                    |
| check_timeout         | Integer         | 30                | Oddychový čas pre _check_command_ v sekundách                                                              |
| retry_interval        | Integer         | 60                | Interval opakovania kontroly v sekundách. Tento interval sa použije když sa _host_ nachádza v stave `SOFT` | 
| templates             | Array[String]   | []                | Šablona obsahujúca preddefinované atribúty                                                                 |
| vars                  | Hash            | {}                | Hash obsahujúci vlastné atribúty                                                                           |

#### Príklad vytvorenia hosta
```puppet
icinga2::host{ $facts['fqdn']:
    check_command        => "hostalive",
    address              => $facts['ipaddress'],
    groups               => ["skupina-cerit"],
    templates            => ["generic-host"],
    enable_notifications => true,
}
```

### Vytvorenie objektu typu _service_
K zavedeniu služby do monitorovania sa využíva definovaný resource `icinga2::service` s nasledujúcimi atribútmi:

#### **`Povinné`** atribúty:

| Názov atribútu        | Typ             | Popis atribútu                         |
|-----------------------|-----------------|----------------------------------------|
|check_command          | String          | reprezentuje názov kontrolného príkazu |


#### Voliteľné atribúty:

| Názov atribútu            | Typ             | Defaultná hodnota | Popis atribútu                    |
|---------------------------|-----------------|-------------------|-----------------------------------|
| enable_notfications       | Boolean         | false             | Povolenie notifikácií             |
| notification_user         | Array[String]   | []                | Upresnenie uživateľov, ktorý majú byť notifikovaný |
| notification_users_groups | Array[String]   | []                | Upresnenie skupín (contact group), ktoré majú byť notifikované |
| notification_templates    | Array[String]   | []                | Šablona                      |
| check_interval            | Integer         | 300               | Interval kontroly v sekundách. Tento interval sa použije keď je služba v stave `HARD` |
| check_timeout             | Integer         | 30                | Oddychový čas pre check_command v sekundách  |
| retry_interval            | Integer         | 60                | Interval opakovania kontroly v sekundách. Tento interval sa použije keď sa služba nachádza v stave `SOFT` |
| templates                 | Array[String]   | []                | Šablona obsahujúca preddefinované atribúty  |
| vars                      | Hash            | {}                | Hash obsahujúci vlastné atribúty            |


#### Príklad vytvorenia služby
```puppet
  icinga2::service { 'check_ssh_via_nrpe':
      check_command => "nrpe",
      vars          => { "nrpe_port" => 5669, "nrpe_command" => 'check_ssh' },
}
```

## Notifikácia
Z predchádzajúceho textu výplýva, že notifikácia je naviazaná na konkrétnu službu tj. pre nastavanie vlastnosti notifikácie je potrebné v `icinga2::service` upraviť atribúty:
* notification_templates
* notification_user
* notification_users_groups


Pre viac informácií viz. [Objekty v icinge2](https://icinga.com/docs/icinga2/latest/doc/09-object-types/)
