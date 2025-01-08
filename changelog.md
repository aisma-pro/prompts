# tabele: call_checklist

as is
```sql
-- auto-generated definition
create table call_checklist
(
    checklist_id    integer default nextval('checklist_parameters_parameter_id_seq'::regclass) not null
        constraint call_checklist_pk
            primary key,
    call_id         integer                                                                    not null
        constraint checklist_parameters_call_id_fk
            references call
            on delete cascade,
    parameter_name  varchar,
    stage           varchar,
    score           integer,
    justification   text,
    quotes          text,
    recommendations text
);

alter table call_checklist
    owner to "user";

create unique index checklist_parameters_parameter_id_uindex
    on call_checklist (checklist_id);
```

to be
```sql
-- auto-generated definition
create table call_checklist
(
    checklist_id    integer default nextval('checklist_parameters_parameter_id_seq'::regclass) not null
        constraint call_checklist_pk
            primary key,
    call_id         integer                                                                    not null
        constraint checklist_parameters_call_id_fk
            references call
            on delete cascade,
    parameter_name  varchar,
    stage           varchar,
    score           integer,
    justification   text,
    quotes          jsonb,
    recommendations jsonb,
    example         jsonb
);

alter table call_checklist
    owner to "user";

create unique index checklist_parameters_parameter_id_uindex
    on call_checklist (checklist_id);
```

# table: call_client_risks

as is
```sql
-- auto-generated definition
create table call_client_risks
(
    risk_id          integer default nextval('client_risks_risk_id_seq'::regclass) not null
        constraint client_risks_pk
            primary key,
    call_id          integer                                                       not null
        constraint client_risks_call_id_fk
            references call,
    risk_description text,
    recommendations  text,
    risk_name        varchar,
    risk_lavel       integer default 0                                             not null,
    quote            text
);

alter table call_client_risks
    owner to "user";

create unique index client_risks_risk_id_uindex
    on call_client_risks (risk_id);
```

to be
```sql
-- auto-generated definition
create table call_client_risks
(
    risk_id          integer default nextval('client_risks_risk_id_seq'::regclass) not null
        constraint client_risks_pk
            primary key,
    call_id          integer                                                       not null
        constraint client_risks_call_id_fk
            references call,
    risk_description text,
    recommendations  jsonb,
    risk_name        varchar,
    risk_lavel       integer default 0                                             not null,
    quote            text
);

alter table call_client_risks
    owner to "user";

create unique index client_risks_risk_id_uindex
    on call_client_risks (risk_id);
```

# table: call_to_action > call_agreements

as is
```sql

-- auto-generated definition
create table call_to_action
(
    cta_id             serial
        constraint call_to_action_pk
            primary key,
    call_id            integer not null
        constraint call_to_action_call_id_fk
            references call,
    actor              varchar,
    action_description text,
    deadline           varchar,
    status             integer
);

alter table call_to_action
    owner to "user";

create unique index call_to_action_cta_id_uindex
    on call_to_action (cta_id);
```

to be
```sql
-- auto-generated definition
create table call_agreements
(
    agreements_id integer default nextval('call_to_action_cta_id_seq'::regclass) not null
        constraint call_to_action_pk
            primary key,
    call_id       integer                                                        not null
        constraint call_to_action_call_id_fk
            references call,
    responsible   varchar,
    description   text,
    deadline      varchar,
    status        varchar default 'pending'::character varying
);

alter table call_agreements
    owner to "user";

create unique index call_to_action_cta_id_uindex
    on call_agreements (agreements_id);
```

# table: call_client_objections
as is
```sql
-- auto-generated definition
create table call_client_objections
(
    objection_id    integer default nextval('client_objections_objection_id_seq'::regclass) not null
        constraint client_objections_pk
            primary key,
    call_id         integer                                                                 not null
        constraint client_objections_call_id_fk
            references call,
    quote           text,
    category        varchar,
    manager_actions text,
    score           integer default 0,
    justification   text,
    recommendations text,
    inefficiency    text
);

alter table call_client_objections
    owner to "user";

create unique index client_objections_objection_id_uindex
    on call_client_objections (objection_id);
```
to be 
```sql
-- auto-generated definition
create table call_client_objections
(
    objection_id         integer default nextval('client_objections_objection_id_seq'::regclass) not null
        constraint client_objections_pk
            primary key,
    call_id              integer                                                                 not null
        constraint client_objections_call_id_fk
            references call,
    quote                text,
    category             varchar,
    score                integer default 0,
    recommendations      jsonb,
    unused_opportunities jsonb,
    objection_resolved   boolean default false,
    evaluation_criteria  jsonb,
    conclusions          text
);

alter table call_client_objections
    owner to "user";

create unique index client_objections_objection_id_uindex
    on call_client_objections (objection_id);
```

---

# create table: checklist

```sql
-- auto-generated definition
create table checklist
(
    checklist_id serial
        constraint checklist_pk
            primary key,
    stage_id     integer
        constraint checklist_checklist_stages_stage_id_fk
            references checklist_stages,
    name         varchar,
    description  varchar,
    execution    jsonb,
    is_enable    boolean,
    instructions text,
    pipelinel_id integer
        constraint checklist_checklist_pipelines_pipeline_id_fk
            references checklist_pipelines
);

alter table checklist
    owner to "user";

create unique index checklist_checklist_id_uindex
    on checklist (checklist_id);


```

# create table: checklist_pipelines

```sql
-- auto-generated definition
create table checklist_pipelines
(
    pipeline_id serial
        constraint checklist_pipelines_pk
            primary key,
    name        varchar
);

alter table checklist_pipelines
    owner to "user";

create unique index checklist_pipelines_pipeline_id_uindex
    on checklist_pipelines (pipeline_id);
```

# create table: checklist_stages

```sql
-- auto-generated definition
create table checklist_stages
(
    stage_id serial
        constraint checklist_stages_pk
            primary key,
    name     varchar
);

alter table checklist_stages
    owner to "user";

create unique index checklist_stages_stage_id_uindex
    on checklist_stages (stage_id);

```