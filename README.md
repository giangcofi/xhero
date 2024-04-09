
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bảng tính điểm X-HERO</title>
    <link rel="icon" href="https://avatars.githubusercontent.com/u/67627577?v=4">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css" rel="stylesheet"
        integrity="sha384-EVSTQN3/azprG1Anm3QDgpJLIm9Nao0Yz1ztcQTwFspd3yD65VohhpuuCOmLASjC" crossorigin="anonymous">
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/js/bootstrap.bundle.min.js"
        integrity="sha384-MrcW6ZMFYlzcLA8Nl+NtUVF0sA7MsXsP1UyJoMp4YLEuNSfAP+JcXn/tWtIaxVXM"
        crossorigin="anonymous"></script>
</head>

<div class="container">
    <h2 class="p-3">Bảng tính điểm rương Kho Báu</h2>
    <form onsubmit="calculate_stellar_points(event, this)">
        <div class="mb-3">
            <label for="blue_treasure" class="form-label" style="color:blue;">Rương Xanh lam</label>
            <input type="number" min="0" class="form-control" id="blue_treasure"
                placeholder="Nhập số lượng Rương Xanh lam">
        </div>
        <div class="mb-3">
            <label for="purple_treasure" class="form-label" style="color:purple;">Rương Tím</label>
            <input type="number" min="0" class="form-control" id="purple_treasure"
                placeholder="Nhập số lượng Rương Tím">
        </div>
        <div class="mb-3">
            <label for="yellow_treasure" class="form-label" style="color:orange;">Rương Vàng</label>
            <input type="number" min="0" class="form-control" id="yellow_treasure"
                placeholder="Nhập số lượng Rương Vàng">
        </div>
        <div class="mb-3">
            <label for="red_treasure" class="form-label" style="color:red;">Rương Đỏ</label>
            <input type="number" min="0" class="form-control" id="red_treasure" placeholder="Nhập số lượng Rương Đỏ">
        </div>
        <button type="submit" class="btn btn-primary">Tính điểm</button>
    </form>
    <div id="result" class="mt-3">

    </div>
    <div id="result_info" class="mt-3">

    </div>
</div>
<script>

    function calculate_stellar_points(event, form) {
        event.preventDefault();

        // stages:
        // 40 - 2
        // 70 - 10
        // 100 - 20
        // 130 - 50

        // when you spend 40 - you will credit 40 points to points_earned and credit 2 points to available_points
        // when you spend 70 - you will credit 70 points to points_earned and credit 12 points to available_points
        // when you spend 100 - you will credit 100 points to points_earned and credit 32 points to available_points
        // when you spend 130 - you will credit 130 points to points_earned and credit 82 points to available_points and cycle starts again

        const required_points_stages = [40, 30, 30, 30];
        const earned_points_stages = [2, 10, 20, 50];

        let treasure_counters = [form.blue_treasure.value, form.purple_treasure.value, form.yellow_treasure.value, form.red_treasure.value];
        let available_points = 0;
        let points_earned = 0;

        for (let c_stage = 0; c_stage < required_points_stages.length; c_stage++) {
            available_points += treasure_counters[c_stage] * earned_points_stages[c_stage]
        }

        while (available_points > 0) {
            for (let c_stage = 0; c_stage < required_points_stages.length && available_points > 0; c_stage++) {

                if (available_points >= required_points_stages[c_stage]) {
                    points_earned = points_earned + required_points_stages[c_stage];
                    available_points = available_points - required_points_stages[c_stage] + earned_points_stages[c_stage];
                    treasure_counters[c_stage]++;
                }
                else {
                    points_earned += available_points;
                    available_points = 0;
                }
            }
        }

        document.getElementById("result").innerHTML = `Bạn sẽ nhận được <b>${points_earned}</b> điểm${points_earned > 0 ? '' : ''}`;
        document.getElementById("result_info").innerHTML =
            `<ul>
                <li>Mở <b style="color:blue;">${treasure_counters[0] || 0} </b> Rương Xanh lam</li>
                <li>Mở <b style="color:purple;">${treasure_counters[1] || 0} </b> Rương Tím</li>
                <li>Mở <b style="color:orange;">${treasure_counters[2] || 0} </b> Rương Vàng</li>
                <li>Mở <b style="color:red;">${treasure_counters[3] || 0} </b> Rương Đỏ</li>
            </ul>`;
    }
</script>
