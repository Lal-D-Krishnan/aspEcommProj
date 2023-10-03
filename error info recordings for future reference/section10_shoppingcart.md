## 133 Fix Issues with Add to cart

```cs

 [HttpPost]
 [Authorize]
 public IActionResult Details(ShoppingCart shoppingCart)
 {
     var claimsIdentity = (ClaimsIdentity)User.Identity;
     var userId = claimsIdentity.FindFirst(ClaimTypes.NameIdentifier).Value;
     shoppingCart.ApplicationUserId = userId;

     ShoppingCart cartFromDb = _unitOfWork.ShoppingCart.Get(u => u.ApplicationUserId == userId &&
     u.ProductId == shoppingCart.ProductId);


     if(cartFromDb != null)
     {
         //shopping cart exists
         cartFromDb.Count += shoppingCart.Count;
         _unitOfWork.ShoppingCart.Update(cartFromDb);
     }
     else
     {
         //add cart record
     _unitOfWork.ShoppingCart.Add(shoppingCart);

     }

     _unitOfWork.Save();
     return RedirectToAction(nameof(Index));
 }

```

## 134 A Weird Bug

Everything works as expected even though the Update Statement is commented out why?.

```cs
 [HttpPost]
 [Authorize]
 public IActionResult Details(ShoppingCart shoppingCart)
 {
     var claimsIdentity = (ClaimsIdentity)User.Identity;
     var userId = claimsIdentity.FindFirst(ClaimTypes.NameIdentifier).Value;
     shoppingCart.ApplicationUserId = userId;

     ShoppingCart cartFromDb = _unitOfWork.ShoppingCart.Get(u => u.ApplicationUserId == userId &&
     u.ProductId == shoppingCart.ProductId);


     if(cartFromDb != null)
     {
         //shopping cart exists
         cartFromDb.Count += shoppingCart.Count;
         _unitOfWork.ShoppingCart.Update(cartFromDb);
     }
     else
     {
         //add cart record
      // _unitOfWork.ShoppingCart.Add(shoppingCart);

     }

     _unitOfWork.Save();
     return RedirectToAction(nameof(Index));
 }
```
