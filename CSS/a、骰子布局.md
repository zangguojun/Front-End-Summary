## 一点

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        body {
            background: #000;
        }

        .main {
            width: 200px;
            height: 200px;
            background: #fff;
            border-radius: 20px;
            margin: 100px auto;
            padding: 25px;
            box-sizing: border-box;

            display: flex;
            justify-content: center;
            align-items: center;
        }

        .main>div {
            width: 40px;
            height: 40px;
            background: #000;
            border-radius: 40px;
        }
    </style>
</head>

<body>
    <div class="main">
        <div class="item"></div>
    </div>
</body>

</html>
```

## 二点

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        body {
            background: #000;
        }

        .main {
            width: 200px;
            height: 200px;
            background: #fff;
            border-radius: 20px;
            margin: 100px auto;
            padding: 25px;
            box-sizing: border-box;

            display: flex;
            flex-direction: column;
            justify-content: space-around;
            align-items:center
        }

        .main>div {
            width: 40px;
            height: 40px;
            background: #000;
            border-radius: 40px;
        }
    </style>
</head>

<body>
    <div class="main">
        <div class="item"></div>
        <div class="item"></div>
    </div>
</body>

</html>
```

## 三点

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        body {
            background: #000;
        }

        .main {
            width: 180px;
            height: 180px;
            background: #fff;
            border-radius: 20px;
            margin: 100px auto;
            padding: 25px;
            box-sizing: border-box;
            display: flex;
        }

        .main>div {
            width: 40px;
            height: 40px;
            background: #000;
            border-radius: 40px;
        }

        .item:nth-child(2) {
            align-self: center;
        }

        .item:nth-child(3) {
            align-self: flex-end;
        }
    </style>
</head>

<body>
    <div class="main">
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
    </div>
</body>

</html>
```

## 四点

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        body {
            background: #000;
        }

        .main {
            width: 200px;
            height: 200px;
            background: #fff;
            border-radius: 20px;
            margin: 100px auto;
            padding: 25px;
            box-sizing: border-box;

            display: flex;
            flex-wrap:wrap;
            align-content:space-between;
        }

        .column>div {
            width: 40px;
            height: 40px;
            background: #000;
            border-radius: 40px;
        }

        .column {
            flex-basis: 100%;
            display: flex;
            justify-content: space-between;
        }
    </style>
</head>

<body>
    <div class="main">
        <div class="column">
            <div class="item"></div>
            <div class="item"></div>
        </div>
        <div class="column">
            <div class="item"></div>
            <div class="item"></div>
        </div>
    </div>
</body>

</html>
```

## 五点

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        body {
            background: #000;
        }

        .main {
            width: 200px;
            height: 200px;
            background: #fff;
            border-radius: 20px;
            margin: 100px auto;
            padding: 25px;
            box-sizing: border-box;

            display: flex;
            flex-wrap:wrap;
            align-content:space-between;
        }

        .column>div {
            width: 40px;
            height: 40px;
            background: #000;
            border-radius: 40px;
        }

        .column {
            flex-basis: 100%;
            display: flex;
            justify-content: space-between;
        }
        .column:nth-child(2){
            justify-content: center;
        }
    </style>
</head>

<body>
    <div class="main">
        <div class="column">
            <div class="item"></div>
            <div class="item"></div>
        </div>
        <div class="column">
            <div class="item"></div>
        </div>
        <div class="column">
            <div class="item"></div>
            <div class="item"></div>
        </div>
    </div>
</body>

</html>
```

## 六点

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        body {
            background: #000;
        }

        .main {
            width: 200px;
            height: 200px;
            background: #fff;
            border-radius: 20px;
            margin: 100px auto;
            padding: 25px;
            box-sizing: border-box;

            display: flex;
            flex-wrap:wrap;
            align-content:space-between;
        }

        .column>div {
            width: 40px;
            height: 40px;
            background: #000;
            border-radius: 40px;
        }

        .column {
            flex-basis: 100%;
            display: flex;
            justify-content: space-between;
        }

    </style>
</head>

<body>
    <div class="main">
        <div class="column">
            <div class="item"></div>
            <div class="item"></div>
        </div>
        <div class="column">
            <div class="item"></div>
            <div class="item"></div>
        </div>
        <div class="column">
            <div class="item"></div>
            <div class="item"></div>
        </div>
    </div>
</body>

</html>
```

## 八点

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        body {
            background: #000;
        }

        .main {
            width: 250px;
            height: 250px;
            background: #fff;
            border-radius: 20px;
            margin: 100px auto;
            padding: 25px;
            box-sizing: border-box;

            display: flex;
            flex-wrap:wrap;
            align-content:space-between;
        }

        .column>div {
            width: 40px;
            height: 40px;
            background: #000;
            border-radius: 40px;
        }

        .column {
            flex-basis: 100%;
            display: flex;
            justify-content: space-between;
        }

    </style>
</head>

<body>
    <div class="main">
        <div class="column">
            <div class="item"></div>
            <div class="item"></div>
        </div>
        <div class="column">
            <div class="item"></div>
            <div class="item"></div>
        </div>
        <div class="column">
            <div class="item"></div>
            <div class="item"></div>
        </div>
        <div class="column">
            <div class="item"></div>
            <div class="item"></div>
        </div>
    </div>
</body>

</html>
```

## 九点

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        body {
            background: #000;
        }

        .main {
            width: 200px;
            height: 200px;
            background: #fff;
            border-radius: 20px;
            margin: 100px auto;
            padding: 25px;
            box-sizing: border-box;

            display: flex;
            flex-wrap:wrap;
            align-content:space-between;
        }

        .column>div {
            width: 40px;
            height: 40px;
            background: #000;
            border-radius: 40px;
        }

        .column {
            flex-basis: 100%;
            display: flex;
            justify-content: space-between;
        }

    </style>
</head>

<body>
    <div class="main">
        <div class="column">
            <div class="item"></div>
            <div class="item"></div>
            <div class="item"></div>
        </div>
        <div class="column">
            <div class="item"></div>
            <div class="item"></div>
            <div class="item"></div>
        </div>
        <div class="column">
            <div class="item"></div>
            <div class="item"></div>
            <div class="item"></div>
        </div>
    </div>
</body>

</html>
```

