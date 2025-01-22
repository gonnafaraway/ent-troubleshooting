<div align="center">
  <picture>
    <img alt="Entgo.io Troubleshooting" src="https://github.com/gonnafaraway/ent-troubleshooting/assets/35832930/0bfbf721-fd8d-4a1d-9895-5b2f03837187" width="40%">
  </picture>
</div>

<br><br>

<h1 align="center">Entgo.io Framework Troubleshooting</h1>
<div align="center">
Help guide for Entgo.io community (https://t.me/entgoru)
</div>
</br>

## Известные проблемы и решения
### edge "promotion" has a field "promotion_id" but it is not holding a foreign key (https://github.com/ent/ent/issues/1417)
* Это особенность ent, когда нет нормальной возможности сделать O2O, в таком случае технически проще описать схему как M2M без ключей Unique у полей в schema и только в этом случае ошибки не будет, иначе сущности со связями будут уникальными и создать вторую и более связь будет невозможно
### Error: loading schema: entc/load: no schema found in: (https://github.com/ent/ent/issues/3864 or another)
* На данный момент ent не поддерживает go версии 1.22, с высокой долей вероятности у вас либо сам компилятор версии 1.22, либо проект. Откатитесь до 1.21.7 и все заработает :)
### entc/load: schema "Entity": mixin "IDMixin": field "id": expect type (func() uuid.UUID) for uuid default value
* Ошибка возникает из-за уже сгенерированных файлов ent с ошибками, необходимо убрать поле Default, запустить go generate, затем снова добавить поле Default, и снова запустить go generate ... , после этого ошибка исчезнет.
### Как создать составной первичный ключ?
```go
func (UserGroup) Annotations() []schema.Annotation {
    return []schema.Annotation{
        entsql.PrimaryKey("user_id", "group_id")
    }
}

func (UserGroup) Fields() []ent.Field {
    return []ent.Field{
        field.Int("user_id"),
        field.Int("group_id"),
    }
}
```
### Как создать связь с указанием нейминга поля, в которое кладутся данные?
* Можно подобрать генерируемое значение поля, первая часть слова будет браться от таблицы, которая является псевдовладельцем сущности-связи, то есть, если мы привязываем яблоки к магазину, то колонка будет названа минимум shop_ и тд
* Второй и более гранулярный - завести поле Ent в Fields чтобы тип идентификатора связываемой сущности совпадал с ним и указать что-то подобное
```go
edge.
			From("shop", Shop.Type).
			Ref("fruit_shop").
			Field("fruit_id").
			Unique().
			Required(),
```
