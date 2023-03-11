### Angular animation使用

##### html

> 路由出口添加监听，使用outlet传递路由信息

```html
<div class="routerContainer" [@routerAnimation]="prepare(outlet)">
        <router-outlet #outlet="outlet"></router-outlet>
</div>
```

##### ts

> 添加 `animations` 动画

```ts
import { RouterOutlet } from '@angular/router';
import { animate, group, query, style, transition, trigger } from '@angular/animations';

@Component({
	...
  animations: [
    trigger("routerAnimation", [
      transition('* <=> *', [
        query(':enter, :leave', [
          style({
            position: 'absolute',
            left: 0,
            top: 0,
            width: '100%',
            opacity: 0,
            transform: 'translateX(100%)',
          }),
        ], { optional: true }),
        query(':enter', [
          animate('0.3s cubic-bezier(0.19, 1, 0.22, 1)', style({ opacity: 1, transform: 'translate(0)' })),
        ], { optional: true })
      ]),
    ])
  ]
})


// 添加监听方法，将下一个路由抛出去
prepare(outlet: RouterOutlet) {
    return (outlet && outlet.activatedRouteData && outlet.activatedRouteData?.['animation'])
}
```

##### app.module.ts

> 导入 `BrowserAnimationsModule`、`BrowserModule` 模块

```ts
import { BrowserModule } from '@angular/platform-browser';
import { BrowserAnimationsModule } from '@angular/platform-browser/animations';

// 引入
@NgModule({
  imports: [
    ...
    BrowserModule,
    BrowserAnimationsModule
  ],
})
```

**参考：**[animate](https://angular.cn/api/animations/animate)
