<template>
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Login</title>
        <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
        <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.2/css/all.min.css" integrity="sha512-SnH5WK+bZxgPHs44uWIX+LLJAJ9/2PkPKZ5QiAj6Ta86w+fsb2TkcmfRyVX3pBnMFcV7oQPJkl9QevSCWr3W6A==" crossorigin="anonymous" referrerpolicy="no-referrer" />
    </head>
         <body class="bg-gradient-to-r from-sky-950 via-neutral-400 to-sky-950 flex justify-center items-center h-screen w-screen">
         <div class= "bg-slate-950 bg-transparen p-20 rounded-xl border-zinc-500 shadow-md w-full h-full" >
            
        <div class="flex justify-center absolute top-0 left-1/2 transform -translate-x-1/2 mt-8">
            <div class="w-20 h-20 flex justify-center items-center bg-blue-800 rounded-full mt-12 ">
            <i class="fa-solid fa-user fa-3x text-black-600 "></i>
    </div>
        </div>

            <form>
        <div class="mb-8 relative">
        <i class=" "></i>
        <div class="mb-4">
         <i class="fa-solid fa-user fa-lg" style="color: #2157b5;"></i>
            <input class=" bg-transparen border-button border-solid outline-none leading-tight focus:outline-none focus:shadow-outline rounded p-1.5 ml-3.5 h-8 w-80 border bg-none bg-zinc-500 text-white" id="username" type="text" placeholder="Usuário" >
    </div>
            </div>
                
            <div class="mb-10">
                <i class="fa-solid fa-lock fa-lg" style="color: #2157b5;" ></i>
                <input class=" bg-transparen border-button border-solid outline-none
                leading-tight focus:outline-none focus:shadow-outline rounded p-3 ml-3.5 h-8 w-80 border bg-none bg-slate-600 text-white" id="username" type="text" placeholder="Senha" >
                </div>
                <button type="submit" class="w-80 h-11 tracking-widest flex justify-center bg-neutral-800 text-white font-serif py-3 px-6 p-1.5 rounded ml-8 mr-0 mx-3 hover:bg-neutral-600 ">LOGIN</button>
                <a href="#" class="text-sky-700 hover:text-sky-900 flex justify-center items-center ml-10 m-5">Criar minha conta</a>
            </form>
        </div>
    </body>
</template>