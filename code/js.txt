let title = document.getElementById("title");
let price = document.getElementById("price");
let taxes = document.getElementById("taxes");
let ads = document.getElementById("ads");
let discount = document.getElementById("discount");
let total = document.getElementById("total");
let count = document.getElementById("count");
let category = document.getElementById("category");
let btn = document.getElementById("calc");
let create = document.getElementById('send');
let deleteallelement = document.getElementById("danger");
let mood = "create";
let temp;
let data;

// جلب المعلومات من LocalStorage أو تهيئة مصفوفة جديدة
if (localStorage.product != null) {
    data = JSON.parse(localStorage.product);
} else {
    data = [];
}

// Get Informtion 
function getInformation() {
    if (price.value !== '' && taxes.value !== '' && ads.value !== '' && discount.value !== '') {
        let result = (+price.value + +taxes.value + +ads.value) - +discount.value;
        total.value = result;
    } else {
        total.value = '';
    }
}
btn.addEventListener("click", getInformation);

//  Clear
create.onclick = function() {
    let newdata = {
        title: title.value,
        price: price.value,
        taxes: taxes.value,
        ads: ads.value,
        discount: discount.value,
        count: count.value,
        category: category.value,
    };

    if (mood === "create") {
        if (newdata.count > 1) {
            for (let i = 0; i < newdata.count; i++) {
                data.push(newdata);
            }
        } else {
            data.push(newdata);
        }
    } else {
        data[temp] = newdata;
        mood = "create";
        create.innerHTML = "Create";
        count.style.display = "block";
    }

    localStorage.setItem("product", JSON.stringify(data));
    clear();
    showData();
};

// Clear 
function clear() {
    title.value = '';
    price.value = '';
    taxes.value = '';
    ads.value = '';
    discount.value = '';
    category.value = '';
    count.value = '';
    total.value = '';
}

// Show Data 
function showData() {
    let table = '';
    for (let i = 0; i < data.length; i++) {
        table += `
            <tr>
                <td>${i + 1}</td>
                <td>${data[i].title}</td>
                <td>${data[i].price}</td>
                <td>${data[i].taxes}</td>
                <td>${data[i].ads}</td>
                <td>${data[i].discount}</td>
                <td>${data[i].count}</td>
                <td>${data[i].category}</td>
                <td><button onclick="update(${i})" class="btn btn-danger">Update</button></td>
                <td><button onclick="Delete(${i})" class="btn btn-dark">Delete</button></td>
            </tr>
        `;
    }
    document.getElementById("tbody").innerHTML = table;
    if (data.length > 0) {
        deleteallelement.style.display = "block";
    } else {
        deleteallelement.style.display = "none";
    }
}

//  Delete One Product
function Delete(i) {
    data.splice(i, 1);
    localStorage.setItem("product", JSON.stringify(data));
    showData();
}

// Delete All  
deleteallelement.onclick = function() {
    localStorage.clear();
    data.splice(0);
    showData();
};

//   update
function update(i) {
    title.value = data[i].title;
    price.value = data[i].price;
    taxes.value = data[i].taxes;
    ads.value = data[i].ads;
    discount.value = data[i].discount;
    category.value = data[i].category;
    count.style.display = "none";
    total.value = (+price.value + +taxes.value + +ads.value) - +discount.value;
    create.innerHTML = "Update";
    mood = "update";
    temp = i;
    scroll({
        top: 0,
        behivour: "smooth"
    })
}
// placeholder title or caregorey
let txt = "title"

function gettitleid(id) {
    let inputel = document.getElementById("search");
    if (id == "serone") {
        txt = 'Title';
        inputel.placeholder = "Serach By Title";

    } else {
        txt = 'Category';
        inputel.placeholder = "Serach By Catgeroy";
    }

}

function searchdata(value) {
    let table = '';
    if (txt == "title") {
        for (let i = 0; i < data.length; i++) {
            if (data[i].title.includes(value)) {
                table += `
                    <tr>
                        <td>${i}</td>
                        <td>${data[i].title}</td>
                        <td>${data[i].price}</td>
                        <td>${data[i].taxes}</td>
                        <td>${data[i].ads}</td>
                        <td>${data[i].discount}</td>
                        <td>${data[i].count}</td>
                        <td>${data[i].category}</td>
                        <td><button onclick="update(${i})" class="btn btn-danger">Update</button></td>
                        <td><button onclick="Delete(${i})" class="btn btn-dark">Delete</button></td>
                    </tr>
                `;
            }
        }
    } else {
        for (let i = 0; i < data.length; i++) {
            if (data[i].category.includes(value)) {
                table += `
                    <tr>
                        <td>${i + 1}</td>
                        <td>${data[i].title}</td>
                        <td>${data[i].price}</td>
                        <td>${data[i].taxes}</td>
                        <td>${data[i].ads}</td>
                        <td>${data[i].discount}</td>
                        <td>${data[i].count}</td>
                        <td>${data[i].category}</td>
                        <td><button onclick="update(${i})" class="btn btn-danger">Update</button></td>
                        <td><button onclick="Delete(${i})" class="btn btn-dark">Delete</button></td>
                    </tr>
                `;
            }
        }
    }

    document.getElementById("tbody").innerHTML = table;
}

// show data
showData();