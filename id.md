# Profil xProf : Identification

---

- [Agents](#agents)
- [Groupes](#groups)
- [Activités](#activities)
- [Profil](#profile)


<a name="agents"></a>
## Agents

### Identification en clair

Les agents peuvent être identifiés en clair de manière provisoire, à titre de démonstration. On s’appuie sur les identifiants remontés par Moodle dans le cadre de l’intégration LTI avec xProf.

``` json
"actor": {
    "objectType": "Agent",
    "name": "Jon Snow",
    "account": {
        "homePage": "http://moodle.isae.fr",
        "name": "jon.snow"
    }
}
```

### Identification anonymisée

En production, on s’appuie sur des identifiants anonymisés, gérés de manière centralisée par un service de gestion des identités (potentiellement le LDAP).

``` json
"actor": {
    "objectType": "Agent",
    "account": {
        "homePage": "http://moodle.isae.fr",
        "name": "8be985de-9d4f-331a-8f06-fd4bab2030f7"
    }
}
```

<a name="groups"></a>
## Groupes

Les groupes d'apprenants sont identifiés de manière stricte par un UUID dans la propriété `account.name`
et de manière plus littéraire dans la propriété `name`.

``` json
"actor": {
    "objectType": "Group",
    "name": "Group 2019/02",
    "account": {
        "homePage": "http://moodle.isae.fr",
        "name": "8be985de-9d4f-331a-8f06-fd4bab2030f7"
    }
}
```

<a name="activities"></a>
## Activités

### Activité source

C'est l'activité pédagogique sur laquelle porte la séance d'évaluation.

- S'il s'agit d'une activité Moodle, elle est identifiée comme précisé dans le profil xAPI Moodle. Exemple : `http://moodle.isae.fr/xapi/activities/lti/1fae9d58-6bc7-42ec-ad9f-f9633d102fef`.

- Dans le cas contraire, un schéma d'identification clair devra être précisé.  


### Learning Outcome

Il s'agit d'un objectif pédagogique associé à l'activité source et devant faire l'objet d'une évaluation dans xProf.

- S'il s'agit d'un Learning Outcome déclaré dans Moodle, l'identification suit le schéma précisé dans le profil xAPI Moodle. Exemple : `http://moodle.isae.fr/xapi/activities/grade_outcomes/1fae9d58-6bc7-42ec-ad9f-f9633d102fef`.

- Dans le cas contraire, un schéma d'identification clair devra être précisé.  


### Séance xProf

Les séances xProf sont identifiées par la plateforme xProf selon le schéma `http://platform_iri/xapi/activities/session/xxx`,
où `http://platform_iri` est l'IRI de la plateforme xProf utilisée. Exemple : `http://xprof.isae.fr/xapi/activities/grade_outcomes/1fae9d58-6bc7-42ec-ad9f-f9633d102fef`.


<a name="profile"></a>
## Profils

- Le profil xProf est identifié par `http://xapi.isae.fr/vocab/profiles/xprof`.

- Le profil VLE est identifié par `http://vocab.xapi.fr/categories/vle-profile`.



