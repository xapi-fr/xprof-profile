# Profil xProf : Statements

---

- [Règles communes](#rules)
- [D01 - Un concepteur a défini un LO.](#D01)
- [T01 - L’enseignant a lancé une séance.](#T01)
- [T02 - L’enseignant a clôturé une séance.](#T02)
- [T03 - L’enseignant a annulé une évaluation.](#T03)
- [L01 - L’apprenant a été évalué sur un LO.](#L01)
- [L02 - L’apprenant a été évalué sur une séance.](#L02)


<a name="rules"></a>
### Règles communes

- Le `type` des activités DOIT être précisé.
- Le profil xProf DOIT être indiqué dans la propriété `context.contextActivities.category`. 
- Lorsque xProf est lancé depuis Moodle, via une activité LTI, la propriété `context.contextActivities.grouping` DOIT mentionner l'instance de la plateforme Moodle, ainsi que le cours et l'activité LTI concernés. 
- La propriété `context.platform` DOIT être précisée et avoir pour valeur `xProf`.
- La propriété `context.team` DOIT identifier le groupe d'apprenants concerné sans lister ses membres.
- La propriété `timestamp` DOIT être précisée.


<a name="D01"></a>
### D01 - Un concepteur a défini un LO.

Cette trace est facultative. 
Elle permet de définir précisemment les LO dès de leur création, avant même qu'ils ne soient utilisés par xProf.
L'intérêt est d'éviter d'avoir à redéfinir dans le détail les LO à chaque évaluation de l'apprenant
lors d'une trace [L01 - L’apprenant a été évalué sur un LO](#L01).

- La propriété `object.definition.correctResponsesPattern` habituellement utilisée pour les interactions CMI de type likert ne DOIT pas être utilisée ici.
- La propriété `object.definition.scale` DOIT définir les niveaux d'évaluation, du moins satisfaisant au plus satisfaisant, correspondant à des scores allant de 0 à N.
- La propriété `context.contextActivities.parent` DOIT préciser l'activité à laquelle est rattaché le LO.
- L'extension de contexte `cmi-interaction-weighting` PEUT être utilisée pour fixer un poids au LO dans le cadre de l'activité.

``` json
{
    "actor": {
        "objectType": "Agent",
        "name": "Sansa Stark",
        "account": {
            "homePage": "http://moodle.isae.fr",
            "name": "sansa.stark"
        }
    },
    "verb": {
        "id": "http://id.tincanapi.com/verb/defined"
    },
    "object": {
        "objectType": "Activity",
        "id": "http://moodle.isae.fr/xapi/activities/grade_outcomes/1fae9d58-6bc7-42ec-ad9f-f9633d102fef",
        "definition": {
            "type": "http://adlnet.gov/expapi/activities/cmi.interaction",
            "description": {
                "fr-FR": "Comprendre les principes de fonctionnement d'un moteur."
            },
            "interactionType": "likert",
            "scale": [
                {
                    "id": "low", 
                    "description": {
                        "fr-FR": "It's really not OK"
                    }
                },
                {
                    "id": "medium", 
                    "description": {
                        "fr-FR": "It could be better"
                    }
                },
                {
                    "id": "hight", 
                    "description": {
                        "fr-FR": "It's OK"
                    }
                }
            ]
        }    
    },
    "context": {
        "contextActivities": {
            "parent": [
                "object": {
                    "objectType": "Activity",
                    "id": "http://xapi.moodle.test/xapi/activities/lti/e403e7ee-4cdd-4d25-b7d9-5de3569a1cc2",
                    "definition": {
                        "type": "http://vocab.xapi.fr/activities/external-activity"
                    }
                }
            ],
            "grouping": [
                {
                    "objectType": "Activity",
                    "id": "http://xapi.moodle.test",
                    "definition": {
                        "type": "http://vocab.xapi.fr/activities/system"
                    }
                },
                {
                    "objectType": "Activity",
                    "id": "http://xapi.moodle.test/xapi/activities/course/ba297687-b1aa-4477-9efd-a782c8fdb90a",
                    "definition": {
                        "type": "http://vocab.xapi.fr/activities/course"
                    }
                }
            ],            
            "category": [
                {
                    "objectType": "Activity",
                    "id": "http://xapi.isae.fr/vocab/profiles/xprof",
                    "definition": {
                        "type": "http://adlnet.gov/expapi/activities/profile"
                    }
                }
            ]
        },
        "platform": "xProf",
        "extensions": {
            "http://id.tincanapi.com/extension/cmi-interaction-weighting": 0.5
        }
    },
    "timestamp": "2019-02-25T15:34:25.804+00:00"
}

```


<a name="T01"></a>
### T01 - L’enseignant a lancé une séance.

- L’extension d’activité `deferred` DOIT préciser si la saisie de la séance est faite immédiatement (`false`) ou de manière différée (`true`).
- L’extension d’activité `duration` DOIT préciser la durée prévue de la séance (en cas de saisie immédiate) ou effective (en cas de saisie différée), en format ISO 8601.
- L’extension d’activité `date` DOIT préciser la date effective de la séance dans le cas d'une saisie différée (format ISO 8601). Elle ne DOIT pas être précisée dans le cas d'une saisie immédiate.

``` json
{
    "actor": {
        "objectType": "Agent",
        "name": "Jon Snow",
        "account": {
            "homePage": "http://moodle.isae.fr",
            "name": "jon.snow"
        }
    },
    "verb": {
        "id": "http://adlnet.gov/expapi/verbs/initialized"
    },
    "object": {
        "objectType": "Activity",
        "id": "http://xprof.isae.fr/xapi/activities/session/123456",
        "definition": {
            "type": "http://vocab.xapi.fr/activities/live-session",
            "extensions": {
                "http://vocab.xapi.fr/extensions/deferred": true,
                "http://id.tincanapi.com/extension/duration": "PT1H",
                "http://id.tincanapi.com/extension/date": "2019-02-25",
            }
        }
    },
    "context": {
        "contextActivities": {
            "grouping": [
                {
                    "objectType": "Activity",
                    "id": "http://xapi.moodle.test",
                    "definition": {
                        "type": "http://vocab.xapi.fr/activities/system"
                    }
                },
                {
                    "objectType": "Activity",
                    "id": "http://xapi.moodle.test/xapi/activities/course/ba297687-b1aa-4477-9efd-a782c8fdb90a",
                    "definition": {
                        "type": "http://vocab.xapi.fr/activities/course"
                    }
                },
                {
                    "objectType": "Activity",
                    "id": "http://xapi.moodle.test/xapi/activities/lti/e403e7ee-4cdd-4d25-b7d9-5de3569a1cc2",
                    "definition": {
                        "type": "http://vocab.xapi.fr/activities/external-activity"
                    }
                }
            ],            
            "category": [
                {
                    "objectType": "Activity",
                    "id": "http://xapi.isae.fr/vocab/profiles/xprof",
                    "definition": {
                        "type": "http://adlnet.gov/expapi/activities/profile"
                    }
                }
            ]
        },
        "team": {
            "objectType": "Group",
            "name": "Group 2019/02",
            "account": {
                "homePage": "http://moodle.isae.fr",
                "name": "8be985de-9d4f-331a-8f06-fd4bab2030f7"
            }
        },
        "platform": "xProf"
    },
    "timestamp": "2019-02-25T15:34:25.804+00:00"
}

```

<a name="T02"></a>
### T02 - L’enseignant a clôturé une séance.

- La propriété `result.score` PEUT être précisée pour laisser une appréciation d'ensemble (les 4 sous-propriétés sont obligatoires).
- La propriété `result.response` PEUT être précisée pour laisser un commentaire d'ensemble.

``` json
{
    "actor": {
        "objectType": "Agent",
        "name": "Jon Snow",
        "account": {
            "homePage": "http://moodle.isae.fr",
            "name": "jon.snow"
        }
    },
    "verb": {
        "id": "http://adlnet.gov/expapi/verbs/terminated"
    },
    "object": {
        "objectType": "Activity",
        "id": "http://xprof.isae.fr/xapi/activities/session/123456",
        "definition": {
            "type": "http://vocab.xapi.fr/activities/live-session"
        }
    },
    "result": {
        "response": "Global comments for this session...",
        "score": {
            "min": 0,
            "max": 4,
            "raw": 2,
            "scaled": 0.5
        }
    },
    "context": {
        "contextActivities": {
            "grouping": [
                {
                    "objectType": "Activity",
                    "id": "http://xapi.moodle.test",
                    "definition": {
                        "type": "http://vocab.xapi.fr/activities/system"
                    }
                },
                {
                    "objectType": "Activity",
                    "id": "http://xapi.moodle.test/xapi/activities/course/ba297687-b1aa-4477-9efd-a782c8fdb90a",
                    "definition": {
                        "type": "http://vocab.xapi.fr/activities/course"
                    }
                },
                {
                    "objectType": "Activity",
                    "id": "http://xapi.moodle.test/xapi/activities/lti/e403e7ee-4cdd-4d25-b7d9-5de3569a1cc2",
                    "definition": {
                        "type": "http://vocab.xapi.fr/activities/external-activity"
                    }
                }
            ],            
            "category": [
                {
                    "objectType": "Activity",
                    "id": "http://xapi.isae.fr/vocab/profiles/xprof",
                    "definition": {
                        "type": "http://adlnet.gov/expapi/activities/profile"
                    }
                }
            ]
        },
        "team": {
            "objectType": "Group",
            "name": "Group 2019/02",
            "account": {
                "homePage": "http://moodle.isae.fr",
                "name": "8be985de-9d4f-331a-8f06-fd4bab2030f7"
            }
        },
        "platform": "xProf"
    },
    "timestamp": "2019-02-25T15:34:25.804+00:00"
}

```

<a name="T03"></a>
### T03 - L’enseignant a annulé une évaluation.

``` json
{
    "actor": {
        "objectType": "Agent",
        "name": "Jon Snow",
        "account": {
            "homePage": "http://moodle.isae.fr",
            "name": "jon.snow"
        }
    },
    "verb": {
        "id": "http://adlnet.gov/expapi/verbs/voided"
    },
    "object": {
        "objectType": "StatementRef",
        "id": "3a4a60dd-7b5c-393f-b96d-294f63c97b90"
    },
    "timestamp": "2019-02-25T15:34:25.804+00:00"
}

```

> {danger.fa-exclamation-triangle} L'utilisation du verbe `void` conduit à l'annulation du Statement dans le LRS.
Les Statements annulés sont susceptibles de disparaitre totalement lors d'un transfert de LRS à LRS.
Cette approche est donc délicate car elle peut conduire à emputer une partie de l'historique de l'apprenant.
Cette trace peut être utilisée suite à une erreur de saisie par le formateur, 
mais pas dans l'optique de ré-évaluer l'apprenant.
Un apprenant qui a été ré-évaué doit disposer de l'historique de ses évaluations successives.



<a name="L01"></a>
### L01 - L’apprenant a été évalué sur un LO.

- Les propriétés `object.definition.description`, `object.definition.interactionType` et `object.definition.scale` DOIVENT être définies si aucune trace de définition du LO n'a été émise précédemment. Se référer à [D01 - Un concepteur a défini un LO](#D01).
- La propriété `result.score` DOIT préciser la note obtenue, avec ses 4 sous-propriétés obligatoires.
- La propriété `result.response` PEUT préciser l'appréciation de l'enseignant.
- La propriété `context.contextActivities.parent` DOIT préciser l'activité parente qui est la séance xProf.
- L'extension de contexte `cmi-interaction-weighting` PEUT être utilisée pour fixer un poids au LO dans le cadre de la séance.

``` json
{
    "actor": {
        "objectType": "Agent",
        "name": "Arya Stark",
        "account": {
            "homePage": "http://moodle.isae.fr",
            "name": "arya.stark"
        }
    },
    "verb": {
        "id": "http://vocab.xapi.fr/verbs/was-assessed"
    },
    "object": {
        "objectType": "Activity",
        "id": "http://moodle.isae.fr/xapi/activities/grade_outcomes/1fae9d58-6bc7-42ec-ad9f-f9633d102fef",
        "definition": {
            "type": "http://adlnet.gov/expapi/activities/cmi.interaction",
            "description": {
                "fr-FR": "Comprendre les principes de fonctionnement d'un moteur."
            },
            "interactionType": "likert",
            "scale": [
                {
                    "id": "low", 
                    "description": {
                        "fr-FR": "It's really not OK"
                    }
                },
                {
                    "id": "medium", 
                    "description": {
                        "fr-FR": "It could be better"
                    }
                },
                {
                    "id": "hight", 
                    "description": {
                        "fr-FR": "It's OK"
                    }
                }
            ]
        }    
    },
    "result": {
        "response": "Comments for a given learner...",
        "score": {
            "min": 0,
            "max": 2,
            "raw": 1,
            "scaled": 0.5
        }
    },
    "context": {
        "contextActivities": {
            "parent": [
                "object": {
                    "objectType": "Activity",
                    "id": "http://xprof.isae.fr/xapi/activities/session/123456",
                    "definition": {
                        "type": "http://vocab.xapi.fr/activities/live-session"
                    }
                }
            ],
            "grouping": [
                {
                    "objectType": "Activity",
                    "id": "http://xapi.moodle.test",
                    "definition": {
                        "type": "http://vocab.xapi.fr/activities/system"
                    }
                },
                {
                    "objectType": "Activity",
                    "id": "http://xapi.moodle.test/xapi/activities/course/ba297687-b1aa-4477-9efd-a782c8fdb90a",
                    "definition": {
                        "type": "http://vocab.xapi.fr/activities/course"
                    }
                },
                {
                    "objectType": "Activity",
                    "id": "http://xapi.moodle.test/xapi/activities/lti/e403e7ee-4cdd-4d25-b7d9-5de3569a1cc2",
                    "definition": {
                        "type": "http://vocab.xapi.fr/activities/external-activity"
                    }
                }
            ],            
            "category": [
                {
                    "objectType": "Activity",
                    "id": "http://xapi.isae.fr/vocab/profiles/xprof",
                    "definition": {
                        "type": "http://adlnet.gov/expapi/activities/profile"
                    }
                }
            ]
        },
        "team": {
            "objectType": "Group",
            "name": "Group 2019/02",
            "account": {
                "homePage": "http://moodle.isae.fr",
                "name": "8be985de-9d4f-331a-8f06-fd4bab2030f7"
            }
        },
        "instructor": {
            "objectType": "Agent",
            "name": "Jon Snow",
            "account": {
                "homePage": "http://moodle.isae.fr",
                "name": "jon.snow"
            }
        },
        "platform": "xProf",
        "extensions": {
            "http://id.tincanapi.com/extension/cmi-interaction-weighting": 0.5
        }
    },
    "timestamp": "2019-02-25T15:34:25.804+00:00"
}

```


<a name="L02"></a>
### L02 - L’apprenant a été évalué sur une séance.

- La propriété `result.response` DOIT préciser l'appréciation de l'enseignant.

``` json
{
    "actor": {
        "objectType": "Agent",
        "name": "Arya Stark",
        "account": {
            "homePage": "http://moodle.isae.fr",
            "name": "arya.stark"
        }
    },
    "verb": {
        "id": "http://vocab.xapi.fr/verbs/was-assessed"
    },
    "object": {
        "objectType": "Activity",
        "id": "http://xprof.isae.fr/xapi/activities/session/123456",
        "definition": {
            "type": "http://vocab.xapi.fr/activities/live-session"
        }
    },
    "result": {
        "response": "Comments for a given learner..."
    },
    "context": {
        "contextActivities": {
            "grouping": [
                {
                    "objectType": "Activity",
                    "id": "http://xapi.moodle.test",
                    "definition": {
                        "type": "http://vocab.xapi.fr/activities/system"
                    }
                },
                {
                    "objectType": "Activity",
                    "id": "http://xapi.moodle.test/xapi/activities/course/ba297687-b1aa-4477-9efd-a782c8fdb90a",
                    "definition": {
                        "type": "http://vocab.xapi.fr/activities/course"
                    }
                },
                {
                    "objectType": "Activity",
                    "id": "http://xapi.moodle.test/xapi/activities/lti/e403e7ee-4cdd-4d25-b7d9-5de3569a1cc2",
                    "definition": {
                        "type": "http://vocab.xapi.fr/activities/external-activity"
                    }
                }
            ],            
            "category": [
                {
                    "objectType": "Activity",
                    "id": "http://xapi.isae.fr/vocab/profiles/xprof",
                    "definition": {
                        "type": "http://adlnet.gov/expapi/activities/profile"
                    }
                }
            ]
        },
        "team": {
            "objectType": "Group",
            "name": "Group 2019/02",
            "account": {
                "homePage": "http://moodle.isae.fr",
                "name": "8be985de-9d4f-331a-8f06-fd4bab2030f7"
            }
        },
        "instructor": {
            "objectType": "Agent",
            "name": "Jon Snow",
            "account": {
                "homePage": "http://moodle.isae.fr",
                "name": "jon.snow"
            }
        },
        "platform": "xProf"
    },
    "timestamp": "2019-02-25T15:34:25.804+00:00"
}

```


