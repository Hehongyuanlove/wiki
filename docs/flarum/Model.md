# Model 模型

flarum Model 部分补充

##　模型关联

???+ "Illuminate\Database\Eloquent\Concerns\HasRelationships"
     - hasOne($related, $foreignKey = null, $localKey = null)<br>
     - hasMany($related, $foreignKey = null, $localKey = null)<br>
     - belongsTo($related, $foreignKey = null, $ownerKey = null, $relation = null)<br>
     - belongsToMany() 

```php
public function parent()
{
    return $this->belongsTo(self::class, 'parent_id');
    return $this->hasMany(self::class, 'parent_id');
    return $this->hasMany(GoodsComment::class, 'goods_id');
}
```

## 模型创建

```PHP
public static function start(User $actor, Array $goods)
{
    $goodsMod          = new self();
    $goodsMod->price = $goods['price'];
    ....

    $goodsMod->setRelation('user', $actor);
    $goodsMod->raise(new GoodsCreate($goodsMod, $actor));

    return $goodsMod;
}
```

## Extend Model

???+ "Flarum Extend Model"
    - belongsTo<br>
    - belongsToMany<br>
    - hasOne<br>
    - hasMany<br>

```php
// 模型关系
(new Extend\Model(File::class))
    ->hasOne('tinyfile', TinyFile::class, 'uuid', 'uuid'),
```